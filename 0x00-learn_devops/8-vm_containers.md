# VMs vs containers

#### preamble
* an overwhelming majority of all deployments is made to linux servers
    
    <img src="cloud.jpeg"/>

> ###### linux is a kernel, not an OS. repeat: linux is a kernel, not an OS

* linux keeps track of four things:
    * memory (RAM) -> which portions of RAM will have which programmes running on them 
    * CPU (processors) -> allocate sufficient processing power/time to a process whether or not it is asynchronous
    * disk -> allocate sufficient space of the disk(s) for each programme that is running 
    * devices -> GPUs and network cards, for example. allocate said resources to processes

* example: chromium, notepad and spotify run on a linux machine (*walk-into-a-bar* joke...)

    ```text
    ________    _______     _______
    chromium    notepad     spotify
    --------    -------     -------

                _____
                linux
                -----

    ___     ___     ____    _______
    cpu     ram     disk    devices
    ---     ---     ----    -------

    ```

* chromium asks for CPU; linux allocates a portion of its total CPU time. chromium asks for memory; linux *inafanya ile kitu*. case applies for the rest of the resources. the rest of the programmes operate the same way
* this arrangement has a problem: processes know about each other, that is, chromium knows about notepad and spotify etc (each can communicate with the rest). spofity, for example, could, say, write to the file `/home/user1/file.txt` and notepad could read from said file. notepad could kill spotify, simply, using the `kill` command. this is not desirable
* better example: python
    * file at `/usr/bin/python` is a executable corresponding to **pyhton v 2.7**. a programme may expect **python v 3.11**. now what? 
    * programme throws an error; it needs 3.6 py, not 2.7
    * what happens when the path rsolves to either one based on what aprogramme needs?
        * well...that is called *cross-talk*. this isis the point of this example: processes know about each other and each can communicate with the rest
* another example: apacheHTTP and nginx listening at port 80
    * both, by default, listen to port 80 TCP
    * what happpens when both are on?
        * good question. *there can be only one* (iykyk); first come (first turned on), first served. the other throws an error and informs you that the port is in use
* here is where VMs and containers come in. they operate in such a way that processes/programmes are siloed w/o being siloed
* diagrammatically...


    ```text
      ________        _______         _______
      chromium        notepad         spotify
      --------        -------         -------

    ____________    ____________    ____________
    vm/container    vm/container     vm/container
    ------------    ------------     ------------

                       _____
                       linux
                       -----

          ___      ___      ____     _______
          cpu      ram      disk     devices
          ---      ---      ----     -------

    ```

* each process/programme runs in its own container. said container communicates with linux which, in turn, communicates with the ~~~big~~~ four
* what happens in this arrangement?
    * glad you asked...
    * each programme has its own version of shared resources (the prog that requires 2.7 py gets 2.7 py; the one that need 3.11 py gets 3.11 py. everyone is happy)
        * each VM/container *contains* a version of system files that the process/programme needs: nothing more, nothing less
        * example: apacheHTTP and nginx listening at port 80. each can open port 80 on its own VM/container; no cock-ups (each thinks it is the only one listening to said port :sunglasses:)
    * 