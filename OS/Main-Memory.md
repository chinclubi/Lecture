#Main Memory

CPU -> Memory -> Hard disk

![](./imgs/main-memory-1.jpg)

* Base เป็นจุดเริ่มต้นว่า Process นั้นๆจะเรื่มทำงานบน Address ไหนของ Memory
* Limit เป็นจุดสิ้นสุดของ Process บน Memory

![](./imgs/main-memory-2.jpg)

##Memory Binding

![](./imgs/address-binding.jpg)

* compile time คือ แปลงจากภาษาชั้นสูงเป็นภาษาเครื่อง ให้เครื่องทำงาน
* load time คือ ไว้จัดการ จัดตำแหน่งของ memory ให้กับ process

##Logical vs Physical Address
        CPU         =>          MMU             =>    Memory
    (logical)   4 (relocation register)     10    (Physical)

* MMU(Memory Management Unit) เป็นตัว Translate ให้ CPU คุยกับ Memory รู้เรื่อง
* ช่วงการแปลงจาก Logical ไป Physical เป็นช่วง Load Time ในช่วง Binding Address

##Dynamic relocation using a relocation register
![](./imgs/Dynamic-relocation.jpg)
* กระบวนการแปลง Logical address ไปเป็น physical address

##Dynamic Linking
* **routine** หมายถึงงาน อ้างถึงได้หลายอย่างตั้งแต่ Method, process บลาๆๆๆ
* **stub** คือตัวเก็บ address ของ routine เก่าๆ

เวลาจะเรียกใช้ Process ใหม่ๆ จะไปเช็คใน stub ก่อน ว่าเคยมีใน memory แล้วหรือยัง ถ้ามีแล้วจะไม่เรียกมาเก็บใน memory ใหม่ ก็ใช้ตัวเดิมไปเลย

**shared libraries** - คือ Dynamic เป็นการทำงานของ process พร้อมๆกันได้หลายๆอย่าง เลยมีการแชร์พวก library ใน RAM เพื่อถ้ามีใครจะใช้อีกก็ไม่ต้องไปโหลดจาก HDD มาใหม่

##Contiguous Allocation
* Process มันหั่นไม่ได้นะเพิ่ลลลลล
* การจัดสรรพื้นที่ให้ติดกัน
* คือการจัดให้ Process ใช้พื้นที่ว่างใน Memory ให้มันเต็มๆๆๆ จากข้างบนก่อน เพื่อจะได้เหลือพื้นที่ว่างอันใหญ่ๆ จะได้มีให้ใช้ Process อื่นได้ง่ายขึ้น ถ้าใช้มั่วๆ มันจะเหลือ space เล็กๆๆๆๆ เต็มเลย จะจัด Process ใหม่ได้ยาก
* ใช้ Process อย่างน้อย 2 อันถึงจะเรียกว่าติดกัน
![](./imgs/Relocation-register.jpg)

##Multiple-partition allocation
* เท่าที่เข้าใจ ก็คือไอ Contiguous แต่เป็น Contiguous แบบมี Process มากมายมหาศาล =3=?
![](./imgs/multiple-allocation.jpg)

##Dynamic Storage-Allocation Problem
* limit register กับ relocation register เพื่อเป็น HardWare ในการจัดการตำแหน่ง physical address ใหม่
* วิธีว่าง Process ใน memory
  * First-fit - เจอช่องว่างพอก็ใส่เลย
  * Best-fit - ขนาดที่เข้าได้พอดีที่สุด
  * Worst-fit - แจกช่องว่างใหญ่สุดไปให้ process
    - เช่น มี process ขนาด 2 ดันจ่าย memory ให้ 1000 ช่อง

##Fragmentation
1. **external** - มีพื้นที่ว่างเล็กๆจำนวนมาก ทำให้ใส่ Process ลงไปไม่ได้เลย ก็เลยต้อง _compaction(บีบอัด)_ ให้เหลือที่เยอะๆ จะได้จัด Process ลงได้
2. **Internal** - Process จะใช้ Memory แค่ 10(Request) แต่ดันไปจอง 12 หรือ มากกว่า(Allocated Memory) ทำให้ใช้ Memory เปลือง ในแต่ละ Process ผิดพลาดน้อย แต่ ถ้าเกิดเยอะๆ จะทำให้เปลืองมาก

##วิธีเปลี่ยน Logical เป็น Physical
- มี 2 วิธี คือ Segmentation กับ Paging

###Segmentation
* ใช้ Hardware 2 ตัว 1.Segment-table base register (STBR) 2. Segment-table length register (STLR)
* เป็น MMU ใช้ในการเปลี่ยน Logical ไป Physical โดยวิธี Segmentation
* การเปลี่ยนจาก Logical address ไป Physical Address ทำได้หลายวิธี
![](./imgs/SegmentationHardware.jpg)

ต้องสร้าง segment table มาก่อนอันแรก เป็นตัวที่เปลี่ยน Logical เป็น Physical Memory <br>
**s/d** - แปลง Logical to Physical _s_ คือ segment number _d_ คือ offset<br>
**limit** - length of segment<br>
**d (offset)** - ขนาดของ process<br>
**Logical จะถูกแปลงที่ S/D แ้ลวจะเอามาเช็คก่อนว่าที่ว่างใน memory ว่างพอให้ใส่หรือเปล่า โดยตัว limit จะเป็น length ของช่องที่จะเอาไปใส่ ส่วน offset คือ length ของ process ที่จะใส่ในช่อง ถ้า offset น้อยกว่าก็จะใส่ลงไปใน memory ได้**

###Paging
* การซอย Process เป็นอันเล็กๆ เรียกว่า Page
* Frame number คือ หมายเลขของช่องใน memory
* เป็น MMU อีกแบบนึง ในการเปลี่ยน Logical เป็น Physical
![](./imgs/paging.jpg)
* page table จะเก็บ Physical Address ที่เคยแปลงไว้ในนี้ ถ้าหากไม่เคยแปลงมาก่อนก็จะทำการแปลงก่อน ซึ่งอาจจะใช้เวลานาน Page Table เลยเป็นตัวเก็บสิ่งที่เคยทำไว้แล้ว
* ถ้าเกิดเราเก็บข้อมูลแบบ Best-fit Worst-fit จะเกิด **External Fragmentation** ได้ วิธีนี้จะใช้แก้ปัญหาดังกล่าว
* **เพราะว่า Paging วิธีนี้จะทำให้ process ซอยแบ่งให้พอดีกับช่อง memory ได้ จะเก็บไว้ตรงไหนก็ได้ ตามภาพข้างล่าง**
![](./imgs/paging-table.jpg)

####Paging with associative
![](./imgs/paging-associate.jpg)
* เนื่องจากไอวิธี paging ธรรมดามันช้ามากๆๆๆๆๆ เพราะการเข้าไปหาใน Page Table มันช้้ามากๆ ต้อง search หาตรงตัวธรรมดา ก็เลยต้องหาอะไรมาเก็บคล้ายๆ cache ชื่อ Associative Memory หรือ translation look-aside buffers (TLBs)
* ถ้า page ต้องการ physical memory ที่มีอยู่ใน TLBs อยู่แล้ว ก็ไม่ต้องเสียเวลาแปลงจาก Logical เป็น Physical แต่จะใช้ Physical ที่มีอยู่ใน TLBs เลย ถ้าเจอจะเรียก TLB hit ถ้าไม่เจอ TLB miss
* Page Table จะหายก็ต่อเมื่อเรารีคอมใหม่ หรือ ปิดคอมใหม่

#####EAT(Effective Access Time)
* Hit ratio(alpha) - อัตราการเจอใน TLB
* Associative Lookup<br>
EAT = เจอใน TLB + ไม่เจอใน TLB
###### Example
alpha = 80%, Associative Lookup = 20ns for TLB search, 100 ns for memory access<br>
EAT = (80/100 x 100) + (20/100 x (100 + 100))<br>
EAT = 120 ns<br>
 - เวลาไม่เจอใน TLB ต้องไปหาใน Page Table อีกรอบนึงเลยต้อง + 100 ns เข้าไปอีก

##Memory Protection
* ใน page table จะมี 2 ค่าที่คอยบอกสถานะ
* valid - ในส่วนนั้นสามารถใช้งานได้
* invalid - ไม่สามารถในส่วนนั้นได้
* valid-invalid bit เอาไว้บอกว่าจะเก็บลงใน frame ได้หรือไม่ได้

##Shared Pages
![](./imgs/shared-pages.jpg)
* process ส่วนไหนที่ทำงานเหมือนกันก็จะแชร์ page เลย ใช้ physical memory ส่วนเดียวกันเลย

##Hierarchical Page Tables
* ทุกอย่างจะผ่านกระบวนการของ TLB ก่อน ถ้าไม่ได้ก็จะทำอะไรพวกนี้ทั้ง 3 ตัว
###Two-Level Page-Table Scheme
![](./imgs/two-level.jpg)
* คือถ้ามี process เยอะสัสๆ index ก็จะยาวไปเรื่อยยๆๆๆๆๆๆ ก็เลยต้องซอยย่อยๆ เพื่อไม่ให้ index มันยาวโคตรๆ จัดเป็นหมวดหมู่ๆไป เหมือน หนังสือถ้ามี 400 บท แต่ถ้าย่อย 400 บทเป็น 10 ตอน ก็จะเหลือแต่ละตอนแค่ 40 บท ทำให้มันง่ายในการ access ค่า
* รูปแบบของ Logical Address แบบใหม่
![](./imgs/two-level-2.jpg)
![](./imgs/two-level-3.jpg)
![](./imgs/two-level-4.jpg)

##Hash Page Table
![](./imgs/hash-table.jpg)
* โดยปกติ ถ้าไม่เจอใน TBL สำหรับครั้งแรกสุด Logical Address จะแปลงไปเป็น Physical Memory หลังจากใช้แล้วมันจะ Hash แล้วเก็บไว้ใน hash table ครั้งต่อๆไป ถ้า miss ใน TBL จะมาหาใน Hash Table ก่อน ถ้ามีแล้วก็จะไปที่ physical memory เดิมที่ hash เก็บไว้เลย
* Hash Table จะเก็บทุกๆครั้งที่มีการแปลง Logical Address เป็น Physical Address ทั้งหมดทุกครั้ง

##Inverted Page
![](./imgs/Inverted-Page.jpg)
* จะใช้ pid ในการหา p (physical address)
* ช้ามากในการหา pid
* แต่จะประหยัด memory มากๆ
* ขนาดของ page table จะมีขนาดเท่ากับ physical memory
* ที่เรียก Inverted เพราะว่า เรารู้ตำแหน่งของ physical address หมดแล้ว เวลาเรียกใช้จึงเป็นการมา search ใน page table ทั้งหมด
