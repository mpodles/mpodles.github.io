---
start_date:
  year: 2022
  month: 4
headline: '<a href="https://www.usenix.org/conference/nsdi22/presentation/shashidhara">FlexTOE</a>'
media:
  url: 'https://www.youtube.com/watch?v=0sRLk2zNrdE'
group: Research
---
<section>
    <h2>Description</h2>
    <p> With the advance of SmartNIC architectures that host wimpy
        multi-threaded CPU cores, the authors propose an architecture that disaggregates
        the TCP stack into parallelizable blocks which can be independantly scaled on
        the small cores. The wimpy cores carry out only the operations of the data-path
        and the more sophisticated logic required for congestion control is done on the
        server's CPU
    </p> 
    <h2>Experiments</h2>
    <p>The architecture is tested against standard Linux stack</p>
</section>
