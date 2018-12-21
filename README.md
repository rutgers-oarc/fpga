# FPGA at Rutgers

## Hardware Specs

PowerEdge R740XD Server: 
- 32 Xeon 2.6Ghz cores 
- 24 slots of 32GB RDIMMs (total 768Gb RAM) 
- 4 FPGA cards [Intel FPGA Programmable Acceleration Card](https://www.intel.com/content/www/us/en/programmable/products/boards_and_kits/dev-kits/altera/acceleration-card-arria-10-gx.html)

## How to get access

- FPGA box is part of ERN (Eastern Regional Network), and hosted at Rutgers. Requests for access should fill out [this form for general Amarel access](https://oarc.rutgers.edu/access/) and specify that you need an account on ERN and that hardware you would like to use is FPGA. 

## Directions to get to the box

- ssh to amarel 
- ssh to a machine called mace. This is a login node for Rutgers part of the ERN slurm federation
- FOR now: ssh to fpga1. (For the future: run this command: `salloc -p fpga ...TBD... `  (in order for slurm to allocate fpga resources to you) )

## Join the community 

- We have a [user forum](https://ask.oarc.rutgers.edu/questions/) - login with your Rutgers credentials

## Resources

- [Parallel Programming for FPGAs book](http://kastner.ucsd.edu/hlsbook/)
- [Intel Acceleration Stack](https://www.intel.com/content/www/us/en/programmable/solutions/acceleration-hub/acceleration-stack.html)
- [Intel FPGA training](https://www.intel.com/content/www/us/en/programmable/solutions/acceleration-hub/knowledge-center.html)
- [List of reference designs](https://www.intel.com/content/www/us/en/programmable/products/design-software/embedded-software-developers/opencl/support.html#ref-designs)
- [5-lecture course material for students](https://software.intel.com/en-us/ai-academy/students/kits/dl-inference-fpga)
- [Intel fpga channel on youtube](https://www.youtube.com/channel/UC0wEPiFb0J6AZZ3oPXRoRpw)
- [Catalog of many FPGA classes](https://www.intel.com/content/www/us/en/programmable/support/training/catalog.html)
- [Intel's open source website/community](https://01.org/)  - you can search for FPGA
- [Open Programmable Acceleration Engine](https://01.org/opae) - has link to github
- [Intel's developer community](https://devmesh.intel.com/)
- [Some interesting projects (not hardware, but was pointed to it at the OpenVINO workshop](https://github.com/IntelLabs) 
- If you need to use FPGAs from Matlab check out [this 32-minute online course](https://www.intel.com/content/www/us/en/programmable/support/training/course/odspintro.html) on DSP Builder. 

## Past Events

- Nov xx, 2018 - AI on PC workshop (for engineering undergraduate students)
- Oct 8, 2018 - FPGA Workshop by Bill Jenkins

