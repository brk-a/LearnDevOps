# VMs vs containers

#### preamble
* an overwhelming majority of all deployments are made to linux servers
    
    <img src="cloud.jpeg"/>


    > ### linux is a kernel, not an OS. repeat: linux is a kernel, not an OS


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
    * what happens when the path rsolves to either one based on what a programme needs?
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

* each process/programme runs in its own container. said container communicates with linux which, in turn, communicates with the ~~big~~ four
* what happens in this arrangement?
    * glad you asked...
    * each programme has its own version of shared resources (the prog that requires 2.7 py gets 2.7 py; the one that needs 3.11 py gets 3.11 py. everyone is happy)
        * each VM/container *contains* a version of system files that the process/programme needs: nothing more, nothing less
        * example: apacheHTTP and nginx listening at port 80. each can open port 80 on its own VM/container; no cock-ups (each thinks it is the only one listening to said port :sunglasses:)

#### how containers work
* TLDR: a container provides a *"fake"* version of linux to the processes that run in it
* they create a *namespace*; a namespace is a feature in linux that bundles shared resources (a *sandbox*, if you like)
    * example: processes running in a docker container are, technically, running  w/i linux itself but would not see the processes running natively (directly) on linux
    * better explanation: use `ps -aux |  wc -l` in a terminal:
        * on linux to see how many processes are running natively on linux (notice that the docker container is listed and counted here, however, the processes running inside it are not)
        * in the docker container to see how many processes are running inside said container (notice the processes running natively on linux are neither listed nor counted here)
    * programmme in a container, container 1, asks,"what are the contents of */usr/lib/python*?"
        * linux does not reply using the actual contents of said path; instead, it uses the contents of */var/lib/docker/overlatyfs/1/usr/lib/python*
        * this wee deception allows programmes to run in parallel (synchronously) because linux responds with different files to each container 
* each container has a custom view of the global file system  based on its configs

#### how VMs work
* TLDR: a VM provides a *"fake"* version of the ~~big~~ four (CPU, RAM, disk & devices) to processes that run on it
    * this "*faking*" is one level deeper than the one performed by a container
    * example: an old(er) video game, say, [dangerous dave][def] running on windows 11 (newest winOS as of Thu, 12 Oct, 2023)

    > ###### context: hypervisor
    > a hypervisor *"lies"* to a VM, VM 1, viz: *there is one SSD attached; it has 50Gb worth of storage*. said VM writes to the *"SSD"*, however, the write goes to a file in the host (the actual computer) at `/etc/disks/disk-1.qcow2`

* when a VM runs a process, said process corresponds to an instance of the hypervisor w/i linux
* a hypervisor is in charge of creating VMs

#### VM vs container

|metric|VM|container|
|:---|:---|:---|
|power|more powerful than containers|converse|
|run an OS|can run any OS|can only be connected to a linux distro|
|HW configs|can run different hardware configs|good luck running HW configs|
|*"lies"*|hypervisor *"lies"* to VM|container *"lies"* to processes|
|nested OS|has a nested OS|no nested OS|
|no ideas|nested OS has no idea that it is not talking to actual HW|processes have no idea that they are not talking to the actual linux distro|
|writes|writes are sent to a file w/i the OS running in the VM; the native linux writes said content to a file on native linux|writes are sent to a file on native linux, not a physical drive|
|CPU speed|10-20% slower than containers|converse|
|storage requirements|50-100% more storage than containers|converse; containers do not need all the files an OS requires|
|memory usage|use *c.* 200Mb of mem for each *"inner OS"*|little to no mem overhead|

#### when are Vms a better choice?
* when you:
    * run untrusted (eg. user-supplied) code
        * unpopular opinion: it is difficult to be confident that they cannot *escape* a container (containers, however, have improved over time)
    * run a guest w/i a linux server
        * example:running windows or mac w/i linux
        * not-a-linux-example: running windows in a mac
    * want to emulate HW devices
        * example: emulate a graphics card for whatever reason
        * not a common use case; has very specific applications in industry 

[def]: https://www.playdosgames.com/online/dangerous-dave/