# Bloaters
คือการที่โค้ดมีการขยายจนมีขนาดใหญ่มากแก้ไขได้ยาก ปกติจะไม่เกิดฉับพลันทันที แต่จะค่อยๆเกิดหลังจากโปรแกรมพัฒนาไปมากแล้ว

## Long Method
### Sign and Symtoms
method มีบรรทัดโค้ดมากเกินไป ปกติไม่ควรยาวกว่า 10 บรรทัด

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/long-method-1.png" />
</p>

### Reason for the Problem
ต้นเหตุคือมีการใส่เข้าไปแต่ไม่ยอมเอาออกมา ซึ่งเกิดจากคนเอาแต่เขียนแต่ไม่ยอมอ่านให้เข้าใจ มันจะไม่รู้สึกตัวจนกระทั่ง method ใหญ่สายเกินแก้
ตามปกติ มันยากกว่ามากที่จะสร้าง Method ใหม่แทนการใส่เพิ่มในของเดิม "เพิ่มแค่ 2 บรรทัดเอง" ไปเรื่อยๆจนเกิดปัญหานี้

### Treatment
เอาโค้ดออกและใส่ใน method ใหม่ แม้ว่าจะแค่บรรทัดเดียวก็ตาม
แม้ว่าจะต้องการคำอธิบายหรือชื่อที่บรรยายไว้แล้ว ก็ไม่มีใครเขาอยากดูโค้ดข้างในว่าทำอะไรหรอก

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/long-method-2.png" />
</p>

- ลดความยาว method ใส้ในได้ด้วยหลักการ Extract Method 
- ถ้ามีค่าตัวแปรที่ยุ่งกับการแยก method ใช้การ Replace Temp with Query, Parameter Object, หรือ Preserve Whole Object
- ถ้าวิธีข้างบนไม่ช่วย ลองย้าย method ทั้งหมดไปเป็นแต่ละ class นึงผ่าน Replace Method with Method Object
- พวก if else และ loop สามารถแยก Method ได้ สำหรับ if else ใช้ Decompose Conditional สำหรับ loop ใช้ Extract Method 

### Payoff
ในโค้ดทั้งหมด class ที่ methods สั้นจะอายุยืนที่สุด, ยิ่งยาวยิ่งยากที่จะรักษาและเข้าใจได้

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/long-method-3.png" />
</p>

### Performance
การมี methods มากไม่ได้มีผลต่อประสิทธิภาพเลย แต่กลับทำให้การทำงานง่ายขึ้นและประสิทธิภาพก็ดีขึ้นอีก

## Large Class
### Sign and Symtoms
class มีจำนวนโค้ดและ method จำนวนมาก

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/large-class-1.png" />
</p>

### Reason for the Problem
class จะเริ่มด้วยจำนวนเล็กๆ แต่จะขยายเมื่อโปรแกรมใหญ่ขึ้น
คล้ายๆกับ Long Methods คนเขียนโค้ดมักเครียดน้อยกว่าถ้าใส่โค้ดไปแทนที่จะสร้างใหม่

### Treatment
คิดถึงการแบ่งแยก

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/large-class-2.png" />
</p>

- Extract Class ช่วยมากถ้าการทำงานสามารถทำเป็นหลายๆองค์ประกอบได้
- Extract Subclass ช่วยมากถ้าการทำงานของ class ใหญ่สามารถดำเนินการในงานที่ไม่ค่อยทำ
- Extract Interface ช่วยถ้ามันจำเป็นจะต้องมีการทำงานที่ลูกค้าสามารถใช้ได้
- ถ้า class จัดการด้าน graphic ก็สามารถลองย้ายข้อมูลและการทำงานบางส่วนไปอีกก้อนต่างหาก ซึ่งอาจจะจำเป็นที่ต้องคัดลอกข้อมูลไว้ 2 ทีและอัพเดทตลอด การ Duplicate Observed Data ช่วยตรงนี้

### Payoff
- การทำแบบนี้จะทำให้นักพัฒนาไม่ต้องจดจำ attributed จำนวนมากใน class
- การแยก class เป็นส่วนๆช่วยหลีกเลี่ยงโค้ดและฟังก์ชั่นที่ซ้ำได้

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/large-class-3.png" />
</p>

## Primitive Obsession
### Sign and Symtoms
- ใช้วิธีดึกดำบรรพ์ทำงาน แทนการทำงานด้วยก้อนเล็กๆ เช่น หน่วยเงิน, ความยาว, หมายเลขโทรศัพท์
- การใช้ค่าคงตัวสำหรับข้อมูลโค้ด เช่น USER_ADMIN_ROLE = 1
- ใช้ตัวอักษรคงตัวเป็นชื่อ field สำหรับใช้ในข้อมูล arrays

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/primitive-obsession-1.png" />
</p>

### Reason for the Problem
เกิดจากจุดอ่อนแอ "แค่ field สำหรับใส่ข้อมูล" นักพัฒนาพูด ทำให้เกิดการสร้าง field ดึกดำบรรพ์ซึ่งง่ายกว่าการสร้าง class ใหม่มาก หลังจากนั้นก็มี field ที่ต้องการและสร้างอีกจนใหญ่เกินจะต้านทาน
โค้ดดึกดำบรรพ์มักจะใช้เพื่อจะจำลองประเภท แทนที่จะแยกประเภทข้อมูลให้ชัด กลับสร้าง string ที่ชื่อเข้าใจง่ายและให้ค่าคงตัว ทำให้มีขนาดใหญ่มาก

### Treatment
ถ้ามีโค้ดดึกดำบรรพ์ขนาดใหญ่และหลากหลาย เป็นไปได้ที่จะแยกกลุ่มเป็นอีก class หรือดีกว่านั้น ย้ายการทำงานที่เกี่ยวกับข้อมูลไปที่ class ด้วย ลอง Replace Data Value with Object

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/primitive-obsession-2.png" />
</p>

- ถ้าค่าใช้ใน method ลอง Parameter Object หรือ Preserve Whole Object
- เมื่อข้อมูลซับซ้อนโค้ดในค่า ใช้ Replace Type Code with Class, Replace Type Code with Subclasses หรือ Replace Type Code with State/Strategy
- ถ้ามี arrays ใช้ Replace Array with Object

### Payoff
- โค้ดยืดหยุ่นมากขึ้นเพราะเป็นกลุ่มก้อนแทนโค้ดดึกดำบรรพ์
- เข้าใจและจัดการง่ายขึ้น
- ค้นพบโค้ดที่ทำซ้ำง่ายขึ้น

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/primitive-obsession-3.png" />
</p>

## Long Parameter List
### Sign and Symtoms
มี parameters มากว่า 3 หรือ 4 ตัวใน method

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/long-parameter-list-1.png" />
</p>

### Reason for the Problem
อาจเกิดขึ้นจาก algorithms หลายจำนวนรวมใน 1 method รายการยาว อาจจะสร้างเพื่อควบคุม algorithm ไหนรันอะไร
รายการยาวอาจจะเกิดจากผลของการสร้าง class ให้เป็นอิสระจากกัน ซึ่งจะต้องมี parameter ของตนเอง ทำให้รายการยาว
มันเข้าใจยาก ทำให้ใช้งานยากเมื่อพัฒนามากขึ้น

### Treatment
เช็คว่าค่าไหนผ่านไปยัง parameters ถ้าบางส่วนเกิดจากการที่ method เรียกอีกอัน ใช้ Replace Parameter with Method Call กลุ่มก้อนนี้จะวางในที่ของแต่ละ class หรือ ส่งผ่าน method ได้

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/long-parameter-list-2.png" />
</p>

- แทนที่จะส่งข้อมูลชุด ส่งกลุ่มก้อนของมันเองไป method แทน ด้วยการ Preserve Whole Object
- ถ้ามีข้อมูลที่ไม่เกี่ยวข้อง เราสามารถรวมให้เป็น parameter ก้อนเดียวได้ด้วย Parameter Object

### Payoff
- โค้ดสั้นและอ่านง่าย
- อาจจะทำให้ค้นพบโค้ดที่ทำซ้ำง่ายขึ้น

### When to Ignore
อย่าทำลาย parameter ถ้าทำแล้วเกิดพันธะที่ไม่ต้องการของแต่ละ class

## Data Clumps
### Sign and Symtoms
- บางครั้งโค้ดในแต่ละส่วนมีค่ากำหนดที่เหมือนกันเลย เช่น parameter สำหรับต่อ database โค้ดกระจุกกลุ่มก้อนเช่นนี้ ควรสร้างเป็น class ของตนเอง

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/data-clumps-1.png" />
</p>

### Reason for the Problem
ข้อมูลกระจุกมักเกิดจากโครงสร้างกากหรือ คัดลอกไม่คิดหน้าคิดหลัง
ถ้าอยากมั่นใจว่าข้อมูลไม่ใช่กระจุก ลองลบทิ้งและดูผลลัพธ์ว่ายังเป็นไปได้ไหม

### Treatment
- ถ้าข้อมูลซ้ำๆใน class ใช้การ Extract Class ไปยัง class ของตนเอง
- ถ้าข้อมูลกระจุกส่งผ่าน parameter ของ method ใช้ Parameter Object เพื่อทำให้เป็น class
- ถ้าข้อมูลบางส่วนส่งผ่าน method คิดถึงการส่งข้อมูลทั้งก้อนไป method แทน ใช้ Preserve Whole Object
- ดูโค้ดให้ดีๆ มันอาจจะย้ายโค้ดนี้ไป class ของข้อมูลได้

### Payoff
- ทำให้การเข้าใจและจัดการดีขึ้นมาก ข้อมูลรวมในที่ๆเดียว
- ลดขนาดโค้ด

<p  align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/data-clumps-3.png" />
</p>

### When to Ignore
ส่งผ่านก้อนข้อมูลใน parameter ของ method แทนที่การส่งผ่านแค่ค่า (ดึกดำบรรพ์) อาจจะสร้างพันธะที่ไม่ต้องการของ 2 class ได้ 