---
title: NVMe kernel tracing
categories:
- General

feature_image: "https://picsum.photos/2560/600?image=733"
---

## Background
My initial reasearch tackles performance problems of NVMe drive emulation. To emulate, generally speaking, is to use software that acts as a hardware device, so that the software that regularly uses some hardware device, like a disk driver, unknowingly is talking to another software appliance rather than actual hardware device. While coming with lower performance, the flexibility due to programmability of an emulator is much greater, allowing for testing new features that could later be turned into electronic components.


In storage systems, linux kernel uses the [blk layer](https://linux-kernel-labs.github.io/refs/heads/master/labs/block_device_drivers.html) when receiving requests from the filesystem and then pushes the request to appropriate block driver. In the case of NVMe, it's dependant on the 

After receiving a IO request the emulated drive is able to perform some logic on the request and forward it further e.g. to a null-blk device or something more sophisticated like drives attached over networked fabric. This can hide all the logic required to perform networking operations from the software using 

with Infiniband, ROCE and TCP being the most known. While debugging the performance of the emulated software, it's important to understand the IO path within the kernel, to find the limitations that might be ocurring there.

Request are submitted using `struct nvme_mq_ops` which holds handlers for specific operations like initializing queues, queueing request, etc.

`nvme_queue_rq` is a function that's  receives a list of requests

```c
kprobe:nvme_queue_rq { @stack[kstack] = count();  }
    nvme_queue_rq+1
    blk_mq_try_issue_list_directly+581
    blk_mq_sched_insert_requests+164
    blk_mq_flush_plug_list+259
    blk_flush_plug_list+221
    blk_finish_plug+45
    __blkdev_direct_IO+1002
    blkdev_direct_IO+83
    generic_file_read_iter+153
    blkdev_read_iter+74
    aio_read+236
    __io_submit_one.constprop.0+267
    io_submit_one+227
    __x64_sys_io_submit+142
    do_syscall_64+89
    entry_SYSCALL_64_after_hwframe+98

```

The methods source:

```c
void blk_mq_try_issue_list_directly(struct blk_mq_hw_ctx *hctx,
                struct list_head *list)
{
  int queued = 0;
  int errors = 0;
 
  while (!list_empty(list)) {
    blk_status_t ret;
    struct request *rq = list_first_entry(list, struct request,
        queuelist);
 
    list_del_init(&rq->queuelist);
    ret = blk_mq_request_issue_directly(rq, list_empty(list));
    if (ret != BLK_STS_OK) {
      if (ret == BLK_STS_RESOURCE ||
          ret == BLK_STS_DEV_RESOURCE) {
        blk_mq_request_bypass_insert(rq, false,
              list_empty(list));
        break;
      }
      blk_mq_end_request(rq, ret);
      errors++;
    } else
      queued++;
  }
 
  /*
   * If we didn't flush the entire list, we could have told
   * the driver there was more coming, but that turned out to
   * be a lie.
   */
  if ((!list_empty(list) || errors) &&
        hctx->queue->mq_ops->commit_rqs && queued)
    hctx->queue->mq_ops->commit_rqs(hctx);
}

```

`blk_mq_request_issue_directly` isn't called and blk_mq_commit_rqs cannot be traced.

```c

kprobe:blk_mq_try_issue_list_directly {printf("blk try\n");}
kprobe:nvme_queue_rq {printf("nvme que\n");

blk try
nvme que
blk try
nvme que
blk try
nvme que
blk try
nvme que
blk try
nvme que
```

`nvme_commit_rqs` is not getting called and `nvme_write_sq_db` is not traceable.
`kprobe:blk_mq_request_issue_directly {@issue_dir=count();}` doesn't trace any function

