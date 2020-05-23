var ss = SpreadsheetApp.openByUrl("https://docs.google.com/spreadsheets/d/1vL-uRhusp5GUfIV7Fu6kFLVueKzydK_VwKyUZlLs3mk/edit"); //ใช้กรณีดึงข้อมูลมาจาก Google Sheet อื่น เพื่อเพิ่ม Script ให้คัดลอก url ของ Google Sheet นั้นมา
var sht1 = ss.getSheetByName("Workday"); //ชื่อชีตที่อยู่ด้านล่าง

function AlertTomorrow(){
//รายงานแจ้งเตือนวันทำงาน Version เค้าเอง
//เป็นการดึงข้อมูลจาก GoogleSheet ออกมาเป็น วัน/เดือน (ไม่มี 0 นำ) เทียบกับวันปัจจุบัน เมื่อถึงเวลาที่ตั้งไว้ก็จะส่งคำสั่งให้ไปแจ้ง
//เตือนที่ Line การใส่ข้อมูลไม่จำเป็นต้องเรียงวันทำงานก่อนหลัง เพราะโปรแกรมจะวนหาและเช็คข้อมูลเอง 
//หากงานเพิ่ม / ลด ก็สามารถทำต่อแถวสุดท้ายได้เลย แต่ต้องไม่มีแถวว่างระหว่างบรรทัด เพราะโปรแกรมวนรอบตามยอดรวม
//=======จัดทำโดย=====
//==นายสมพงษ์  โพคาศรี==
//มือถือ : 095-6659190  email: spkorat0125@gmail.com  Line: guytrue  ****อย่าลบนะ เผื่อไว้ปรึกษาครู****
//=โรงเรียนหนองกราดวัฒนา ต.หนองกราด อ.ด่านขุนทด จ. นครราชสีมา 30210 โทรศัพท์ 044-973561

//สามารถเพิ่มอิโมจิสวย ๆ  ได้จากเว็บไซต์  https://getemoji.com/


var ss=SpreadsheetApp.getActive() //กำหนดใช้ url ของชีตใน ss
var token1=sht1.getRange(2, 7).getValue();  //เซลล์ที่นำตัวแปร token id ของไลน์เป็นห้องที่ต้องการแจ้งเตือน
var txtTitle=sht1.getRange(3, 7).getValue();  //ข้อความนำของไลน์ห้อง 1
var CheckTitle=sht1.getRange(3, 8).getValue();  //ดึงค่าว่า ใช้หรือไม่ใช้  ของไลน์ห้อง 1
var token2=sht1.getRange(5, 7).getValue();  //เซลล์ที่นำตัวแปร token id ของไลน์เป็นห้อง 2 ที่ต้องการแจ้งเตือน  //(ถ้าไม่ต้องการแจ้งห้อง 2 ให้ใส่เครื่องหมาย // หน้าบรรทัดนี้)
var token3=sht1.getRange(7, 7).getValue();  //เซลล์ที่นำตัวแปร token id ของไลน์เป็นห้อง 2 ที่ต้องการแจ้งเตือน  //(ถ้าไม่ต้องการแจ้งห้อง 3 ให้ใส่เครื่องหมาย // หน้าบรรทัดนี้)
  
  
var msgtxtTitle=""; //ตั้งค่าตัวแปรเก็บข้อความว่าง
if (CheckTitle==true){ // เช็คว่าให้ใช้ข้อความนำหรือไม่
      msgtxtTitle=txtTitle+"\n"; //ถ้าใช้ให้ส่งค่าไปที่ msgtxtTitle และขึ้นบรรทัดใหม่ 1 บรรทัด
    }
else{
      msgtxtTitle=""; //ถ้าไม่ใช้ให้ว่าง
}

var strWeek = ["อาทิตย์", "จันทร์", "อังคาร", "พุธ", "พฤหัสบดี", "ศุกร์", "เสาร์"]; //ฟังชก์ชันหาวันในสัปดาห์
var strMonthFull = ["มกราคม", "กุมภาพันธ์", "มีนาคม", "เมษายน", "พฤษภาคม", "มิถุนายน", "กรกฎาคม", "สิงหาคม", "กันยายน", "ตุลาคม", "พฤศจิกายน", "ธันวาคม"]; //ฟังก์ชันเดือน
     
//***กำหนดข้อมูลวันที่ปัจจุบัน***
var  d = new Date(); //กำหนด d เก็บวันที่วันนี้
d.setDate(d.getDate() + 2); //***แจ้งเตือนของวันถัดไป ถ้าต้องการให้เตือนล่วงหน้ากี่วันก็บวกเข้าไป (ถ้าแจ้งเตือนวันนี้เลย จะไม่มีบรรทัดนี้)******
var curdate=d.getDate(); // วันที่ 1-31
var daycur = d.getDay(); // วัน 0-6 เพื่อไปค้นหาวัน จันทร์-ศุกร์
var monthcur=d.getMonth();  //เลขเดือน 0-11 เพื่อไปหา มกราคม-ธันวาคม
var yearcur =d.getFullYear()+543; //เพิ่มปี ค.ศ. ให้เป็น พ.ศ.
var MonthId = strMonthFull[monthcur]; //เลือกเดือนภาษาไทย มกราคม-ธันวาคม
var Day_List = strWeek[daycur]; //เลือกวันของสัปดาห์

var Curvalue1=sht1.getRange(2,2).getValue(); //ยอดรวมที่ต้องวนรอบของงานทั้งหมด

var txtmonth =String(monthcur+1); //ตั้งค่าเดือน +1 เพราะเดือนแรกเป็น 0 แต่เรามักเริ่มนับมักราคมเป็น 1 และตั้งค่าเป็นข้อความ
var txtday=String(curdate); //วันทีปัจจุบัน่ 1-31 ตั้งค่าให้เป็นข้อความ
var txtyear=String(yearcur);// =d.getFullYear()+543; //เพิ่มปี ค.ศ. ให้เป็น พ.ศ.
var txtd_m = txtday+ "/" +txtmonth+ "/" +txtyear ; //นำวันและเดือนมาผสมกัยเป็น  วัน/เดือน เพื่อใช้เทียบค่า 

var FoundWork ="No"; //ตั้งค่าตัวแปรการแจ้งว่าไม่พบก่อนเลย

if (Curvalue1>0) { //ถ้าค่า วัน/เดือน ไม่ว่างหรือมากกว่า 0 ให้ตรวจได้
  var i,j=0; //ตั้งค่าตัวแปร i และ j เป็น 0   
  var msgtotal=""; //สร้างตัวแปรข้อความรวมให้ว่างไว้ก่อน
   for (i = 1; i <= Curvalue1; i++) { //วนรอบจากคนแรกถึงคนสุดท้าย                  
          var datacheck1=sht1.getRange(i+3, 3).getValue(); //เทียบค่ากับวันทำงานของแต่ละคน
          if (txtd_m == datacheck1) { //ถ้าค่าตรงกัน 
              var datacheck2=sht1.getRange(i+3, 2).getValue(); //ให้ดึงชื่อมา
            var datacheck3=sht1.getRange(i+3, 4).getValue(); //ให้ดึงเลขที่งานมา
              var datacheck4=sht1.getRange(i+3, 5).getValue(); //ให้ดึงสถานะมา
              j=j+1; //เพิ่มลำดับ
              msgtotal=msgtotal+"\n\n"+ j + ") " + datacheck2+"\n"+ datacheck3+"    "+ datacheck4; //ใส่ลำดับ หน้าชื่อ รวมกับข้อความเดิม ไปเรื่อยๆ
              FoundWork ="Yes"; //ตั้งค่าว่ามีการพบข้อมูลแล้ว              
         }
      } 
   }

if (FoundWork!="No") { //ส่วนตรวจสอบ ถ้าพบว่ามีงานที่ต้องทำ 
  //ออกแบบข้อความที่ต้องการส่ง การเชื่อมต่อกันใช้ + การขึ้นบรรทัดใหม่ใช้ \n   
    var msg =msgtxtTitle+"\n"+"😂" +  msgtotal + "\n\n\n😈 ผอ.รอตรวจงานคุณภายใน"+"\n"+"วัน"+ Day_List + " ที่ " + curdate + ' ' +MonthId + ' พ.ศ. ' + yearcur           
    
    sendLineNotify(msg,token1);  //สั่งแจ้งเตือนห้องที่ 1
    sendLineNotify(msg,token2); //สั่งแจ้งเตือนห้องที่ 2 //(ถ้าไม่ต้องการแจ้งห้อง 2 ใหใส่เครื่องหมาย // ที่หน้าบรรทัดนี้)
    sendLineNotify(msg,token3); //สั่งแจ้งเตือนห้องที่ 3 //(ถ้าไม่ต้องการแจ้งห้อง 3 ใหใส่เครื่องหมาย // ที่หน้าบรรทัดนี้)
  
  //การแจ้งเตือนห้องที่ 2 และ 3 จะต้องแก้บรรทัดที่ 21 และ 76 กับ 22 และ 77 
  
}
} //จบฟังก์ชัน


function sendLineNotify(message,token) { //ชุดฟังก์ชันแจ้งเตือนไลน์
    var options = {
        "method": "post",
        "payload": "message=" + message,
        "headers": {
            "Authorization": "Bearer " + token
        }
    };
 
    UrlFetchApp.fetch("https://notify-api.line.me/api/notify", options);
}
