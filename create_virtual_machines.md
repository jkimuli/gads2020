## Google Africa Developer Scholarship 2020 - Cloud Track

### Introduction

Lab - Creating Virtual Machines. In this lab I  explored a number of  Virtual Machine instance options and create several VMs with different characteristics with Cloud Shell and Cloud SDK.

### Lab Instructions

* Created a utility virtual machine with the following characteristics
    * Region - us-central1
    * Zone   - us-central1-c
    * Machine Type - n1-standard-1 ( 1vCPU and 3.75GB memory)
    * Machine OS - Debian Linux
    * No external IP 

    * Steps
    
       * Create the VM
        ```console
           student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute  instances create my-vm-1 \ --project=qwiklabs-gcp-04-fab87f85e18e  --zone=us-central1-c --machine-type=n1-standard-1 --subnet=default --no-address 
        ``` 
       * Show that VM has been created
        ```console
            student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute instances list
            NAME     ZONE           MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP  STATUS
            my-vm-1  us-central1-c  n1-standard-1               10.128.0.2                RUNNING
        ```    
        
*  Create a Windows VM with the following characteristics
    * Region - europe-west2
    * Zone - europe-west2-a
    * Machine Type - n1-standard-2 ( 1vCPU and 7.5GB memory)
    * OS - Windows Server 2016 Datacenter Core
    * Firewall allowing HTTP and HTTPS Traffic
    * 100GB SSD persisent disk

    * Steps
      
      * Create the VM
      ```console
         student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute instances create my-windows-vm \ --project=qwiklabs-gcp-04-fab87f85e18e --zone=europe-west2-a --machine-type=n1-standard-2 --subnet=default --network-tier=PREMIUM \ --tags=http-server,https-server --image=windows-server-2016-dc-core-v20200813 --image-project=windows-cloud --boot-disk-size=100GB \
         --boot-disk-type=pd-ssd --boot-disk-device-name=my-windows-vm
      ```

      * Show that the VM has been created
      ```console
         student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute instances list
         NAME           ZONE            MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP  STATUS
         my-windows-vm  europe-west2-a  n1-standard-2               10.154.0.2   34.89.17.4   RUNNING
         my-vm-1        us-central1-c   n1-standard-1               10.128.0.2                RUNNING 
       ``` 

      *  Create Firewall rules for the VM
         ```console
            student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute --project=qwiklabs-gcp-04-fab87f85e18e firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0  \ --target-tags=http-server
            Creating firewall...⠹Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-04-fab87f85e18e/global/firewalls/default-allow-http].
            Creating firewall...done.
            NAME                NETWORK  DIRECTION  PRIORITY  ALLOW   DENY  DISABLED
            default-allow-http  default  INGRESS    1000      tcp:80        False
         ```

         ```console
            student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute --project=qwiklabs-gcp-04-fab87f85e18e firewall-rules create default-allow-https --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:443 --source-ranges=0.0.0.0/0 \ --target-tags=https-server
            Creating firewall...⠹Created [https://www.googleapis.com/compute/v1/projects/qwiklabs-gcp-04-fab87f85e18e/global/firewalls/default-allow-https].
            Creating firewall...done.
            NAME                 NETWORK  DIRECTION  PRIORITY  ALLOW    DENY  DISABLED
            default-allow-https  default  INGRESS    1000      tcp:443        False   
          ```

* Create a Custom VM with following characteristics
    * Region - us-west1
    * Zone   - us-west1-b
    * Machine Type - Custom ( 6vCPU and 32GB memory)
    * Machine OS - Debian Linux


    * Steps

        * Create the VM
            ```console
               student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute  instances create my-custom-vm \ --project=qwiklabs-gcp-04-fab87f85e18e --zone=us-west1-b --machine-type=custom-6-32768 --subnet=default --network-tier=PREMIUM 
            ```

        * Confirm that the VM has been created
              ```console
                student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute instances list
                NAME           ZONE            MACHINE_TYPE    PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
                my-windows-vm  europe-west2-a  n1-standard-2                10.154.0.2   34.89.17.4     RUNNING
                my-vm-1        us-central1-c   n1-standard-1                10.128.0.2                  RUNNING
                my-custom-vm   us-west1-b      custom-6-32768               10.138.0.2   35.227.174.58  RUNNING 
              ```

        * SSH into VM to confirm the VM characteristics
              ```console
                 student_04_cbb30071dbfd@cloudshell:~ (qwiklabs-gcp-04-fab87f85e18e)$ gcloud compute ssh my-custom-vm --zone=us-west1-b
              ```

              ```console
                 student-04-cbb30071dbfd@my-custom-vm:~$ free
                 total        used        free      shared  buff/cache   available
                 Mem:       32943884      221080    32591316        8500      131488    32389532
                 Swap:             0           0           0   
              ```

              ```console
                 student-04-cbb30071dbfd@my-custom-vm:~$ lscpu
                Architecture:        x86_64
                CPU op-mode(s):      32-bit, 64-bit
                Byte Order:          Little Endian
                Address sizes:       46 bits physical, 48 bits virtual
                CPU(s):              6
                On-line CPU(s) list: 0-5
                Thread(s) per core:  2
                Core(s) per socket:  3
                Socket(s):           1
                NUMA node(s):        1
                Vendor ID:           GenuineIntel
                CPU family:          6
                Model:               79
                Model name:          Intel(R) Xeon(R) CPU @ 2.20GHz
                Stepping:            0
                CPU MHz:             2200.000
                BogoMIPS:            4400.00
                Hypervisor vendor:   KVM
                Virtualization type: full
                L1d cache:           32K
                L1i cache:           32K
                L2 cache:            256K
                L3 cache:            56320K
                NUMA node0 CPU(s):   0-5
                Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology non
                stop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single pti ssbd ibrs ibpb
                stibp fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm rdseed adx smap xsaveopt arat md_clear arch_capabilities  
                ```

 




