
صفحه اصلی
طراحی سایت
وردپرس
برنامه نویسی
آندروید
ابزارها
درباره ما
پرتال پشتیبانی مشتریان
Advertisement

 banner2.jpg
آموزش ساخت ربات تلگرام با PHP
5 ماه قبل2,737 تعداد بازدید ها29 نظر

آموزش ساخت ربات تلگرام در php

ابتدا باید در بوت فادر ربات خود را ایجاد کنید .

telegrambotphp1

همان طور که در تصویر بالا می بینید من رباتی به نام myphpbot ایجاد کردم ، توجه داشته باشید که این ربات در پایان آموزش از بین خواهد رفت پس از token id من استفاده نکنید . اگر با بوت فادر کار نکرده اید می توانید به لینک مراجعه کنید .

استفاده ازwebHoock :
در این آموزش قصد داریم از webHook  استفاده کنیم ، برای این کار شما نیاز به مجوز ssl دارید ، که می توانید آن را خریداری کرده یا از سرویس دهندهای رایگان مانند www.cloudflare.com استفاده کنید .

چگونه از www.cloudflare.com ssl رایگان دریافت کنیم ؟
ابتدا به این سایت مراجعه کنید ، سپس یک حساب کاربری برای خود ایجاد کنید.

سپس add site را بزنید ، صفحه ای مانند تصویر زیر خواهید دید .

telegrambotphp2



نام دومین خود را وارد کنید مثلا salam.com و دکمه Begin Scan را بزنید ، دامنه شما اسکن می شود ، اگر همه چیز خوب پیش برود شما وارد صفحه مدیریت DNS ها می شود که به صورت اتوماتیک کارهای مورد نیاز را انجام داده است ، تصویری شبیه تصویر زیر:

telegrambotphp3

برروی دکمه ادامه کلیک کنید و به مسیر خود ادامه دهید ، صفحه ای با تایتل update name servers به شما نمایش داده خواهد ، که در آن name servers فعلی و name servers که باید به جای آن قرار دهید را به شما نمایش داده است ، به پنل مدیریت دامنه خود بروید و در قسمت مدیریت DNS این name servers های جدید را جایگذین قدیمی ها کنید .

telegrambotphp4

بعد از اینکه جابجایی را انجام دادید ، به cloudflare باز گردید قسمت overview را مشاهده کنید و ببینید وضعیت شما چیست ؟

telegrambotphp5

خوب شما می توانید وضعیت ssl را از حالت full  به flexible تغییر دهید برای این کار به قسمت crypto بروید ، در قسمت مربوط به ssl  حالت full  را به flexible تغییر دهید .

telegrambotphp6

صبر کنید تا مانند تصویر بالا active certificate برای شما نمایان شود بعد از این حدود ۴ ساعت صبر کنید تا ssl شما فعال شود .دقت کنید مدت زمان اعتبار رایگان شما ۶ ماه است.

خوب وقتی ssl شما فعال شد بقیه کار ها بسیار ساده است ، ابتدا باید به تلگرام بگویید وب هوک را روی چه آدرسی ست کند به شکل زیر.

https://api.telegram.org/bot273759407:AAEOqgo3I5nob3jIGzkY1ihguhPbXl945RQ/setwebhook?url=https://salam.com/myphpbot.php

دقت کنید که بعد از کلمه bot باید token خود را وارد کنید ، هم چنین در قسمت url آدرس خود را.

اکر همه چیز درست باشد یک json مانند شکل زیر به شما نمایش داده خواهد شد .

telegrambotphp8



وب هوک هم ست شد حالا نوبت نوشتن کدها می رسد ، ربات ما قرار است کار بسیار ساده ای انجام دهد ، اگر کاربر پیام salam را بفرستد در جواب خواهد گفت salam be ruye mahet در غیر این صورت هر پیام دیگری دریافت کند می گوید chi migi??

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
<?php
ini_set('error_reporting', 'E_ALL');
 
$botToken = "273759407:AAEOqgo3I5nob3jIGzkY1ihguhPbXl945RQ";
$webSite = "https://api.telegram.org/bot" . $botToken;
 
$update = file_get_contents("php://input");
$update = json_decode($update, TRUE);
 
$chatId = $update["message"]["chat"]["id"];
$message = $update["message"]["text"];
 
switch ($message) {
 
 case "/start":
 sendMessage($chatId, "شروع می کنیم");
 break;
 case "salam":
 sendMessage($chatId, "salam be ruye mahet");
 break;
 default:
 sendMessage($chatId, "chi migi ??");
 
}
 
 
function sendMessage($chatId, $message)
{
 
 $url = $GLOBALS['webSite'] . "/sendMessage?chat_id=" . $chatId . "&text=" . urlencode($message);
 file_get_contents($url);
 
 
}
 
?>
