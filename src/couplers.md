# Couplers :couple:

## Code Smell แบบน้ำพึ่งเรือเสือพึ่งป่า

### มารู้จักกับ Coupling คืออะไร ?

Coupling คือ ความสัมพันธ์ หรือ การพึ่งพาอาศัยกันของ Class ต่างๆ ในระบบ หากระบบใดมีการพึ่งพาอาศัยกันของ object ที่สูงจะทำให้มีความอิสระที่ต่ำ ดังนั้นการพัฒนาระบบนั้นควรที่จะ
พยายามลดการ coupling ให้น้อยลง และเพิ่มความสอดคล้องกันภายใน class ให้มากขึ้นหรือที่เรียกว่า High Cohesion Low Coupling โดยที่ class เหล่านั้นควรแบ่งหน้าที่อย่างชัดเจน
ยกตัวอย่างเช่น class A มีความสัมพันธ์แบบพึ่งพากับ class B ที่แน่นแฟ้นเกินไปเหมือนแฟน ( Couple ) ที่ไปไหนก็ต้องไปด้วยกันทุกที่ ถ้าต่อมา A มีการเปลี่ยนแปลง โดยเพิ่ม Parameter เข้าไป
จะส่งผลให้ B ก็ต้องเพิ่ม Parameter เข้าไปด้วยซึ่งจะดีกว่าไหมถ้าทั้งสอง class จะมีอิสระจากกันแก้ A ไม่ต้องตามไปแก้ B อีก

### ข้อเสียของ Coupling 
Code Smell แบบนี้ จะทำให้ความเกี่ยวข้องกันระหว่าง Class ต่างๆมีมากเกินไป อาจจะทำให้เวลาทำการย้าย หรือ แก้ไข ก็ทำได้ลำบาก ในอนาคตเมื่อมีการแก้ไขโค้ดนั้นแล้วอาจจะทำให้กระทบไปทั้งระบบ 
ซึ่งระบบที่ดีควรออกแบบให้แต่ละ class หรือ module มีความเป็นอิสระกันให้มากที่สุด เพื่อให้โค้ดมีความยืดยุ่นสามารถรองรับการเปลี่ยนของ requirement ในอนาคตได้
+ ถ้ามีการเปลี่ยนแปลงโครงสร้างใดๆขึ้นมา อาจมี impact กับ Object อื่นๆ ที่ reference ด้วย 
+ เกิดการ Reuse Code ได้ยาก เพราะมี Component ที่เกี่ยวข้องกันเยอะ
+ เวลาทำ Test  ทำได้ยาก เพราะมี Component ที่เกี่ยวข้องกันเยอะ

Smell ที่จะกล่าวต่อไปนี้มีส่วนทำให้เกิดการ coupling ระหว่าง class หรือแสดงให้เห็นว่าจะเกิดอะไรขึ้นหากการ coupling นั้นถูกแทนที่ด้วยการ delegate ที่มากเกินไป

## Feature Envy
### สัญญาณและอาการ
method ใน class เรามีการเข้าถึงข้อมูลใน class อื่นๆ ที่ไม่ใช่ class เราเอง

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/feature-envy-1.png" />
</p>

### สาเหตุของปัญหา
กลิ่นนี้จะเกิดขึ้นก็ต่อเมื่อ ใน class เรามี method ที่เรียกไปยัง class อื่นๆเพื่อเข้าถึงข้อมูลหรือการทำงาน จนทำให้โค้ดมีความเกี่ยวข้องกัน หากเกิดขึ้นแรกๆของการพัฒนาจะยังไม่เป็นอะไร
แต่ถ้าพัฒนาต่อไปเรื่อยๆจนระบบใหญ่ขึ้น เวลาที่ต้องกลับมาแก้โค้ดละก็อาจแก้อีกที่ได้ bug ผุดขึ้นเป็นดอกเห็ดเต็มไปหมด ข้อเสียของ Feature Envy คือจะเกิดสิ่งที่ control ไม่ได้ใน
การหา impact เพื่อแก้ไข code ยิ่งมี smell นี้เยอะเท่าไร ก็ต้อง refactoring code มากขึ้นเท่านั้น
</br>




<p align="center">
  <img src="https://miro.medium.com/max/699/1*rB0Fkdgyc5fk1qNzNafnWw.png" />
</p>

###### T Masileerungsri. (2019, August 18). เหม็นกว่าตด ก็ Code Smell ใน Project นะครับเอ่อ. Retrived from https://medium.com/@thanawatmasileerungsri/code-smell-in-project-dde7572f3636

### การบำรุงรักษา

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/feature-envy-3.png" />
</p>

+ Move method ย้าย method ไปยังที่เหมาะสม ไม่ได้ใช้ข้อมูลใน class ของตัวเองก็ควรย้ายไปอยู่ class อื่นซะ  
+ Extract method สกัดเอาโค้ดส่วนที่มีการใช้ข้อมูล class อื่นออกมาสร้าง method แยกไว้



### ผลตอบรับ
+ ลดการซ้ำซ้อนของโค้ด
+ จัดการโค้ดได้ดีขึ้น

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/feature-envy-2.png" />
</p>

## Inappropriate Intimacy
### สัญญาณและอาการ
ในหนึ่ง class มีการใช้ internal field และ method จาก class อื่นๆ

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/inappropriate-intimacy-1.png" />
</p>

### สาเหตุของปัญหา
ในบางครั้ง class ต่างๆ อาจมีความใกล้ชิดสนิทสนมกันมากไป เรียกใช้ข้อมูลจาก class อื่น โดยไม่จำเป็นหรือเยอะเกิน 
และมีการเข้าถึง private field ของกันและกันเกินพอดี การออกแบบ class ที่ดีควรที่จะรู้เรื่องของกันและกันให้น้อยที่สุด
ซึ่งจะทำให้ง่ายต่อการบำรุงรักษาและ reuse

### การบำรุงรักษา
+ Extract Class
+ Move Method และ Move Field
+ เปลี่ยนจาก Bidirectional Association มาเป็น Unidirectional
+ เปลี่ยนจากการ Inherit มาใช้การ Delegate (ใช้งานคนอื่น)แทน

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/inappropriate-intimacy-2.png" />
</p>

### ผลตอบรับ
+ ช่วยให้ปรับปรุงจัดการโค้ดได้ดีขั้น
+ ลดความซับซ้อน และ reuse โค้ด

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/inappropriate-intimacy-3.png" />
</p>



## Message Chains
### สัญญาณและอาการ
ในโค้ดของเรามีการเรียกใช้ method ต่อกันเป็นทอดๆ ยกตัวอย่างเช่น ```foo.getBar().getFoo().getThis().getThat().doSomething();```

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/message-chains-1.png" />
</p>


### สาเหตุของปัญหา
ห่วงโซ่ข้อความเกิดขึ้นเมื่อ client request object อื่น object นั้นจะ request object อื่นต่อกันเป็นห่วงโซ่ หมายความว่า client ขึ้นอยู่กับตามโครงสร้าง class การเปลี่ยนแปลงใด ๆ จำเป็นต้องปรับเปลี่ยน client
ตาม มีการเรียกใช้ method ต่อๆกันจำนวนมากทำให้โค้ดไม่ยืดหยุ่น เพราะขึ้นอยู่กับความสัมพันธ์ระหว่าง object 

### การบำรุงรักษา
+ Extract Method 
+ Hide Delegate
+ Move Method
+ ควรพิจารณาว่าควรจะยุบตรงไหน โดยที่ไม่ให้ค่าเปลี่ยน

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/message-chains-2.png" />
</p>

### ผลตอบรับ
+ สามารถลดการพึ่งพาระหว่าง class

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/message-chains-3.png" />
</p>


## Middle Man
### สัญญาณและอาการ
class ทำหน้าที่เพียงอย่างเดียวโดย delegate กับ class อื่นๆ

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/middle-man-1.png" />
</p>

### สาเหตุของปัญหา
การ delegate นั้นเป็นสิ่งที่ดีและเป็นหนึ่งในคุณสมบัติพื้นฐานที่สำคัญของ object แต่สิ่งที่ดีมากเกินไปอาจนำไปสู่ object ที่มีหน้าที่เพียงแค่ส่งข้อความไปยัง object อื่นๆ

### การบำรุงรักษา
+ ลบ class ที่เป็น Middle Man 

### ผลตอบรับ
+ ช่วยลดปริมาณของโค้ดลง

<p align="center">
  <img src="https://sourcemaking.com/images/refactoring-illustrations/2x/middle-man-2.png" />
</p>


## Incomplete Library Class
### สัญญาณและอาการ
เมื่อ library ที่เราใช้ไม่ตอบสนองความต้องการของผู้ใช้อีกต่อไป ทางออกเดียวสำหรับปัญหานี้คือการเปลี่ยน library

### สาเหตุของปัญหา
ผู้เขียน library ไม่ได้พัฒนา features ที่เราต้องการใช้งานแล้วหรือเลิกที่จะพัฒนาต่อ สิ่งเหล่านี้เกิดขึ้นเมื่อเรามี responsibility ใหม่แล้วต้องย้ายไปที่ library class 
แต่เราไม่สามารถที่จะแก้ไข library class เพื่อให้รองรับ responsibility ใหม่เหล่านี้ได้

### การบำรุงรักษา
+ Introduce Foreign Method สำหรับการเปลี่ยนแปลงเพียงแค่ไม่กี่ method ที่เกิดขึ้นใน library class
+ Introduce Local Extension สำหรับการเปลี่ยนแปลงใหญ่ๆที่เกิดขึ้นใน library class

### ผลตอบรับ
+ ลดการซ้ำซ้อนของโค้ด

