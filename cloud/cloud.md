### What are some of reasons to access a remote computer system?

-   Your computer does not have enough resources to run the desired
    analysis (memory, processors, disk space, network bandwidth).
-   You want to produce results faster than your computer can.
-   You cannot install software in your computer (application does not
    have support for your operating system, conflicts with other
    existing applications)


### Distributed System Definitions and stacks:

(Note that many definitions exist for these terms)

-   Distributed application: an application that can be executed on a
    distributed system platform (e.g., mpiBLAST)
-   Distributed system platform: software layers that facilitates
    coordination and management of a distributed system (e.g.,
    queue-based system, and MapReduce)

-   High Performance Computing (HPC): large assemble of physical
    machines and a homogeneous operating system (e.g., your
    institutions' HPC, XSEDE's HPC)
-   Cloud Computing: virtual machines, distributed platforms and/or
    applications offered as a service (e.g., Amazon Web Services,
    Microsoft Azure, Google Cloud Computing)

-   Virtual machine (VM): software computer that like a physical
    computer can run an operating system and applications
-   Operating system (OS): the basic software layer that allows
    execution and management of applications
-   Physical machine: the hardware (processors, memory, disk
    and network)

### HPC vs. Cloud:

<table style="width:19%;">
<colgroup>
<col width="8%" />
<col width="11%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">HPC</th>
<th align="left">Cloud</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">User account on the system</td>
<td align="left">root account on the system</td>
</tr>
<tr class="even">
<td align="left">Limited control of the system</td>
<td align="left">Full control of the system</td>
</tr>
<tr class="odd">
<td align="left">Central shared file system</td>
<td align="left">Local file system</td>
</tr>
<tr class="even">
<td align="left">Jobs submitted into a queue</td>
<td align="left">Jobs executed on each resource</td>
</tr>
<tr class="odd">
<td align="left">Account-based isolation</td>
<td align="left">OS-based isolation</td>
</tr>
<tr class="even">
<td align="left">Batch-oriented execution of applications</td>
<td align="left">support for batch or interactive applications</td>
</tr>
<tr class="odd">
<td align="left">Request for resource and time allocation</td>
<td align="left">Pay-per-use</td>
</tr>
<tr class="even">
<td align="left">etc.</td>
<td align="left">etc.</td>
</tr>
</tbody>
</table>

![HPC vs.
Cloud](https://raw.githubusercontent.com/datacarpentry/cloud-genomics/gh-pages/lessons/images/HpcVsCloud.png)

### Resources:

-   Cloud computing offerings:
-   Amazon EC2: <http://aws.amazon.com/ec2/>
-   Microsoft Azure: <https://azure.microsoft.com/en-us/>
-   Google Cloud Platform: <https://cloud.google.com/>
-   CyVerse (was iPlant) Atmosphere:
    <http://www.cyverse.org/atmosphere>
-   CyVerse Atmosphere help page:
    <https://pods.iplantcollaborative.org/wiki/display/atmman/Atmosphere+Manual+Table+of+Contents>
-   HPC offerings:
-   XSEDE: <https://www.xsede.org/high-performance-computing>
