---
title: Debugging
description: and the ways to do this
categories:
- General

---

During my reserach of the networking stack I frequently come up agains problems that can be effectively tackled via different debugging methods. It is then important to remember the methods available so that one is chosen quickly and approprietly. Most of the time, it's not obvious which one of methods will allow observing just the correct data for making the analysis.

Methods can be boiled down to:
 - debugger
 - logger
 - eBPF
 - writing:
    - Excalidraw
    - pen & paper
    - textual

The types of debugging data that can be gathered are:
 - event log - a temporal record of states of the program e.g. packet dump 

Debugger can lose it's usefulness when trying to peek into the data path, where it's not obvious what conditions are being searched for while the amount of states that the application goes through is large. It becomes impossible to step through a large amount of code searching for unexpected behavior.

In those situations, having a printed log of states of the program is considerably more useful, as one can search through it and quickly compare different state-transitions to locate possible problems.
