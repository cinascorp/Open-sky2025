
Towards Real-Time 3D Air Traffic Visualization
Using HTTP/3, Web Workers, and WebGL
Cina Nagshbandi
Independent Researcher, Iran
cinascorp@gmail.com
September 2, 2025
Abstract
The growing demand for real-time air traffic monitoring requires data
delivery and visualization technologies that minimize latency and maxi-
mize scalability. While the OpenSky Network provides a rich source of
ADS-B data, existing browser-based visualization approaches often suffer
from delays caused by REST polling, TCP handshake overhead, and the
lack of parallel processing in the rendering pipeline.
This work proposes a novel architecture for real-time air traffic visual
ization leveraging three key technologies:(1) HTTP/3 (QUIC) for low-
latency and loss-tolerant data streaming, reducing round-trip times com-
pared to HTTP/2 or WebSocket-over-TCP solutions;(2) Web Workers
to process incoming ADS-B and traffic data streams in parallel without
blocking the main UI thread; and (3) WebGL/Three.js for efficient,
GPU-accelerated 3D visualization of aircraft trajectories directly in the
browser.
The proposed solution is designed to display thousands of concurrent
aircraft with smooth frame rates and sub-second latency, even under high
data load. We present a prototype implementation that connects to the
OpenSky live API, decodes positional updates, and renders them as dy-
namic 3D objects on a global map. Preliminary performance tests show
significant reductions in end-to-end latency and improved frame stability
compared to traditional polling-based approaches.
This contribution aims to spark discussion within the OpenSky re-
search community about adopting modern web transport protocols and
browser rendering techniques for large-scale, open aviation data visualiza
tion. Future work will extend the prototype to support predictive trajec-
tory visualization, anomaly detection, and collaborative filtering of traffic
events in real time.
OpenSky Network, HTTP/3, QUIC, WebGL, Web Workers, Real-Time Vi-
sualization, ADS-B
یک معماری جدید​ برای مصورسازی بلادرنگ ترافیک هوایی سه‌بعدی با استفاده از داده‌های هیبریدی ADSB و API
چکیده
این مقاله یک معماری نوین سیستمی به نام «TAR» را برای مصورسازی بلادرنگ و مبتنی بر مرورگر ترافیک هوایی ارائه می‌دهد. این سیستم با بهره‌گیری از یک مدل اکتساب داده هیبریدی و فناوری‌های مدرن وب، بر مشکلات تأخیر و عملکردی روش‌های سنتی غلبه می‌کند. 
داده‌ها از گیرنده‌های محلی ADS-B (مانند RTL-SDR و RTL-TCP) و APIهای عمومی (مانند شبکه‌های OpenSky و Flightradar24) به دست می‌آیند.
معماری پیشنهادی از پروتکل HTTP/3 (QUIC) برای جریان داده با تأخیر کم و از Web Workers برای پردازش موازی و غیرمسدودکننده داده‌ها استفاده می‌کند. 
رندرینگ سه‌بعدی با شتاب‌دهی GPU نیز با ترکیب WebGL و Google 3D Maps API به دست می‌آید. نمونه اولیه پیاده‌سازی‌شده، نرخ فریم روان و تأخیر کمتر از یک ثانیه را حتی تحت بار داده‌ای بالا نشان می‌دهد که این امر پیشرفت قابل‌توجهی را در مقایسه با روش‌های سنتی مبتنی بر نظرسنجی (polling) به اثبات می‌رساند.
نام پروژه «TAR» از واسط کاربری محبوب متن‌باز tar1090 گرفته شده است و نشان‌دهنده تکامل از یک ابزار صرفاً محلی به یک پلتفرم مصورسازی بلادرنگ و یکپارچه جهانی است.
1. مقدمه
1.1. بیان مسئله و انگیزه پژوهش
حجم رو به رشد ترافیک هوایی نیاز به سیستم‌های قدرتمند و مقیاس‌پذیر برای نظارت بلادرنگ را ضروری می‌سازد.
 راه‌حل‌های موجود مبتنی بر مرورگر اغلب با چالش‌های مهمی روبرو هستند، از جمله تأخیر بالا ناشی از پروتکل‌های انتقال داده ناکارآمد (مانند نظرسنجی‌های مکرر REST و سربار اضافی TCP) و گلوگاه‌های عملکردی در سمت کاربر که می‌تواند منجر به مسدود شدن واسط کاربری شود.
این مسائل تجربه کاربری را به‌شدت کاهش می‌دهند، به‌ویژه در سناریوهایی که نیاز به به‌روزرسانی‌های سریع و همزمان هزاران شیء متحرک وجود دارد. در نتیجه، یک معماری پایان به پایان جدید مورد نیاز است که بتواند این محدودیت‌ها را برطرف کرده و مسیری را برای مصورسازی در مقیاس بزرگ داده‌های باز هوانوردی ترسیم کند. این پژوهش با پیشنهاد یک معماری جامع، به این نیاز پاسخ می‌دهد.
1.2. معرفی پروژه 'TAR'
راه‌حل پیشنهادی، یک پروژه تحقیقاتی با عنوان «TAR» است. انتخاب این نام تعمدی بوده و از واسط کاربری وب بسیار پرکاربرد tar1090 برای رمزگشاهای ADS-B الهام گرفته شده است. در حالی که tar1090 یک واسط کاربری وب بهبودیافته برای نمایش محلی داده‌ها ارائه می‌دهد، «TAR» این مفهوم را از یک نمای محلی، تک‌منبعی و دوبعدی، به یک پلتفرم مصورسازی سه‌بعدی، چندمنبعی و بلادرنگ گسترش می‌دهد. این نام به طور نمادین، یک سیر تکاملی را از یک ابزار کاربردیِ محلی به یک معماری پیشرفته برای پردازش و نمایش داده‌های هوانوردی در مقیاس جهانی نشان می‌دهد.
1.3. نوآوری‌های کلیدی و ساختار مقاله
مهمترین نوآوری‌های این پژوهش شامل موارد زیر است: الف) یک مدل اکتساب و تلفیق داده هیبریدی، ب) پذیرش HTTP/3 برای جریان داده با تأخیر کم، ج) استفاده از Web Workers برای پردازش موازی و د) ادغام Google 3D Maps با WebGL برای رندرینگ سه‌بعدی با عملکرد بالا. بخش‌های بعدی این مقاله به تشریح دقیق این اجزای معماری، از جمع‌آوری داده‌ها تا رندرینگ سه‌بعدی و تجزیه و تحلیل عملکرد، خواهند پرداخت.
3. پیشینه و کارهای مرتبط
2.1. فناوری ADSB و شبکه‌های نظارتی غیرمتمرکز
پایه و اساس بسیاری از سیستم‌های ردیابی ترافیک هوایی، فناوری ADS-B (Automatic Dependent Surveillance–Broadcast) است که در آن هواپیماها به طور خودکار، موقعیت، سرعت، ارتفاع و سایر اطلاعات پرواز خود را بر روی فرکانس 1090 مگاهرتز منتشر می‌کنند. این سیگنال‌ها می‌توانند توسط دستگاه‌های مقرون‌به‌صرفه رادیو نرم‌افزارمحور (SDR) مانند RTL-SDR دریافت شوند.
 شبکه‌هایی مانند OpenSky Network با تجمیع داده‌ها از هزاران گیرنده زمینی داوطلب در سراسر جهان، یک منبع داده باز و غنی را برای جامعه پژوهشی فراهم می‌کنند.
این مدل غیرمتمرکز دارای مزایای قابل‌توجهی است که از جمله آن‌ها می‌توان به پوشش جهانی، افزونگی و مقاومت در برابر نقاط شکست تکی اشاره کرد. با این حال، یک تحلیل عمیق‌تر از این رویکرد، به محدودیت‌های مهمی نیز اشاره می‌کند. کیفیت داده‌ها در این شبکه‌ها می‌تواند به دلیل کالیبراسیون متفاوت سخت‌افزار، عدم ثبات فرکانس و تداخل‌های محلی فرکانس رادیویی (RFI) متغیر و ناسازگار باشد. این موضوع نشان می‌دهد که هر سیستمی که تنها بر این منابع تکیه کند، باید از قابلیت‌های قوی اعتبارسنجی و تلفیق داده برخوردار باشد.
2.2. روش‌های دریافت داده‌های برخط (cURL and APIs)
پروژه «TAR» داده‌ها را علاوه بر گیرنده‌های محلی، از دو API مهم دریافت می‌کند: OpenSky Network و Flightradar24. هر دو منبع از طریق درخواست‌های cURL قابل دسترسی هستندAPI شبکه OpenSky برای مقاصد تحقیقاتی و غیرتجاری طراحی شده و بردار وضعیت هواپیماها (موقعیت، سرعت و غیره) را فراهم می‌کند. 
در مقابل، Flightradar24 یک مدل مبتنی بر اشتراک پولی دارد که لایه‌های داده‌ای مختلفی از جمله داده‌های زنده و تاریخی را در اختیار قرار می‌دهد
استفاده از چندین منبع داده نه تنها برای افزونگی بلکه برای ایجاد یک مجموعه داده جامع‌تر و قابل اعتمادتر ضروری است.
یک گیرنده محلی، محدوده پوشش محدودی دارد ، در حالی که APIهای مرکزی پوشش جهانی را ارائه می‌دهند. با این حال، ترکیب این منابع ناهمگون به یک مکانیزم پیچیده تلفیق داده نیاز دارد. این فرآیند باید بتواند جریان‌های داده نامتقارن را مدیریت، رکوردهای تکراری را حذف  و ناسازگاری‌ها (مانند تفاوت در موقعیت گزارش‌شده) را حل کند. بنابراین، یک ماژول قوی تلفیق داده، یک جزء حیاتی در معماری «TAR» است.
2.3. پروتکل‌های جریان داده بلادرنگ (HTTP/3 vs. WebSocket)
در کاربردهای بلادرنگ، WebSockets اغلب به عنوان راه‌حل پیش‌فرض استفاده می‌شوند، زیرا یک کانال ارتباطی دوطرفه بر روی یک اتصال TCP فراهم می‌کنند.
با این حال، اتکا به TCP می‌تواند منجر به پدیده «مسدود شدن سر خط» (head-of-line blocking) شود؛ جایی که از دست رفتن یک بسته داده، تمامی جریان‌های داده دیگر را در همان اتصال متوقف می‌سازد. این محدودیت به شدت بر عملکرد و تأخیر در محیط‌های شبکه‌ای ناپایدار تأثیر می‌گذارد.
پروژه «TAR» با انتخاب HTTP/3، که بر اساس پروتکل QUIC ساخته شده است، این محدودیت را هدف قرار می‌دهد. QUIC از UDP استفاده می‌کند و امکان مالتی‌پلکسینگ (چندگانه سازی) بومی را فراهم می‌کند، به این معنی که جریان‌های داده مستقل از یکدیگر منتقل می‌شوند. بنابراین، از دست رفتن داده‌های مربوط به یک هواپیما بر جریان داده‌های سایر هواپیماها تأثیر نمی‌گذارد.
مزایای QUIC—مانند کاهش تأخیر از طریق دست‌دهی (handshake) سریع 0-RTT، برقراری اتصال کارآمدتر، و مهاجرت بی‌درنگ بین شبکه‌ها (مثلاً از Wi-Fi به داده سلولی) —به طور خاص برای کاربردی مانند ردیابی ترافیک هوایی که ممکن است در محیط‌های متحرک یا شبکه‌های ناپایدار مورد استفاده قرار گیرد، بسیار مناسب است. 
این انتخاب فنی نه تنها یک نوآوری محسوب می‌شود، بلکه مستقیماً به چالش‌های مربوط به «عملکرد سیستم/شبکه» که توسط کمیسیون بررسی عملکرد EUROCONTROL مطرح شده، می‌پردازد.
| ویژگی | HTTP/3 (QUIC) | WebSocket |
| پروتکل لایه حمل | UDP  | TCP  |
| مسدود شدن سر خط | ندارد (جریان‌های مستقل)  | دارد (در صورت از دست رفتن بسته)  |
| سربار دست‌دهی | پایین (دست‌دهی 0-RTT/1-RTT)  | بالا (دست‌دهی TCP + TLS + WebSocket)  |
| مهاجرت شبکه | پشتیبانی بومی (بدون قطع اتصال)  | نیاز به برقراری مجدد اتصال  |
| کنترل ازدحام | در فضای کاربر (انعطاف‌پذیر)  | در فضای هسته (سفت و سخت)  |
| ماهیت اتصال | Stateful (در سطح QUIC)  | Stateful (در سطح TCP)  |
2.4. فناوری‌های مصورسازی سه‌بعدی تحت وبWebGL (Web Graphics Library) یک API جاوا اسکریپت است که امکان رندرینگ گرافیک‌های سه‌بعدی با شتاب‌دهی GPU را در مرورگر بدون نیاز به پلاگین فراهم می‌کند. کتابخانه‌هایی مانند Three.js، پیچیدگی‌های WebGL را ساده‌سازی می‌کنند و توسعه‌دهندگان را قادر می‌سازند تا صحنه‌های سه‌بعدی را با سرعت بیشتری ایجاد کنند.
پروژه «TAR» با اتکا به Google 3D Maps، یک رویکرد استراتژیک را در پیش می‌گیرد.
این پلتفرم یک راه‌حل یکپارچه با نقشه‌های سه‌بعدی فوتورئالیستیک، رندرهای داخلی، نشانگرهای سفارشی و قابلیت افزودن مدل‌های سه‌بعدی فراهم می‌کند. این تصمیم، یک تعادل دقیق بین کنترل کامل و سرعت توسعه ایجاد می‌کند. 
در حالی که یک راه‌حل مبتنی بر WebGL/Three.js خالص (مانند پروژه Flight Stream ) حداکثر انعطاف‌پذیری را ارائه می‌دهد، استفاده از Google Maps به عنوان یک پایه قوی، توسعه را سرعت می‌بخشد. 
با استفاده از ویژگی WebGL Overlay View در Google Maps JavaScript API، پروژه «TAR» یک بستر آماده و دقیق جغرافیایی را با امکان رندرینگ محتوای سه‌بعدی سفارشی (مانند مدل‌های هواپیما) ترکیب می‌کند. این رویکرد، یک راه‌حل قدرتمند و در عین حال عملی را برای مصورسازی بلادرنگ فراهم می‌آورد.
3. معماری و پیاده‌سازی سیستم 'TAR'
3.1. جمع‌آوری داده‌های هیبریدی
معماری «TAR» بر یک مدل اکتساب داده هیبریدی استوار است. در سمت سرور، یک ماژول جمع‌آوری داده با سه منبع ورودی تعامل دارد. اول، خروجی داده‌های JSON از یک گیرنده محلی ADS-B، مانند یک Raspberry Pi مجهز به RTL-SDR و نرم‌افزار readsb، به صورت بلادرنگ دریافت می‌شود. دوم، ماژول با استفاده از cURL به صورت دوره‌ای از API شبکه OpenSky Network برای دریافت بردار وضعیت تمامی هواپیماهای در حال پرواز درخواست می‌دهد. سوم، درخواس
ت‌های مشابهی به API Flightradar24 ارسال می‌شود تا داده‌های تکمیلی و تجاری به دست آید. این ماژول مرکزی داده‌های هر سه منبع را دریافت، پردازش اولیه و برای مرحله تلفیق آماده می‌کند.
| منبع داده | نوع داده | مزایا | معایب |
| گیرنده محلی ADS-B | موقعیت، سرعت، ارتفاع، کد ICAO  | تأخیر بسیار کم، داده‌های خام، کنترل کامل  | پوشش محدود (140-480 کیلومتر)  |
| شبکه OpenSky | بردار وضعیت هواپیما  | داده‌های باز، پوشش جهانی، مناسب برای تحقیق  | برای استفاده غیرتجاری، کیفیت داده متغیر  || Flightradar24 | 
موقعیت، مشخصات پرواز، اطلاعات تاریخی پوشش جهانی، داده‌های غنی و تأییدشده  | مدل مبتنی بر اشتراک پولی، هزینه‌های احتمالی 
3.2. پردازش و مدیریت داده‌ها
الگوریتم تلفیق داده: برای تجمیع داده‌ها از منابع ناهمگون، یک الگوریتم پیچیده تلفیق داده ضروری است. در این معماری، از بردار وضعیت هواپیما که با کد یکتای ICAO آن شناسایی می‌شود، به عنوان کلید اصلی استفاده می‌شود.
یک الگوریتم فیلتر تخمین وضعیت مانند فیلتر فدرال کالمن  می‌تواند داده‌های موقعیتی، سرعتی و ارتفاعی را از منابع مختلف با دقت‌های متفاوت ترکیب کند تا یک بردار وضعیت "تلفیق‌شده" و قابل اعتمادتر به دست آید. 
سیستم می‌تواند به داده‌های محلی ADS-B به دلیل تأخیر کمتر، اولویت دهد اما از داده‌های API برای پر کردن خلأها در پوشش استفاده کند.
حذف داده‌های تکراری و یکپارچگی داده: جلوگیری از ایجاد چندین شیء هواپیمای یکسان، یک چالش اساسی است. یک مکانیزم قوی برای شناسایی و حذف داده‌های تکراری با استفاده از شناسه یکتای ICAO24 و کنترل مهرهای زمانی ضروری است.
یک لایه حافظه پنهان (caching layer) مانند Redis با یک زمان انقضای مشخص  می‌تواند به سرعت بررسی کند که آیا یک رکورد در یک بازه زمانی معین پردازش شده است یا خیر و از پردازش مجدد آن جلوگیری کند.
پردازش موازی با Web Workers: برای جلوگیری از مسدود شدن ریسه اصلی (main UI thread) که مسئول رندرینگ است، عملیات سنگین پردازش داده به یک Web Worker منتقل می‌شود. ریسه اصلی داده‌های خام چندمنبعی را با استفاده از متد postMessage() به worker ارسال می‌کند. اسکریپت worker تمامی منطق تلفیق داده، نرمال‌سازی و حذف تکرار را در پس‌زمینه انجام می‌دهد و سپس داده‌های تمیز و آماده رندر را به ریسه اصلی بازمی‌گرداند. این فرآیند تضمین می‌کند که واسط کاربری روان و پاسخگو باقی بماند، حتی زمانی که هزاران هواپیما در حال پردازش هستند.
4. چارچوب مصورسازی بلادرنگ سه‌بعدی
4.1. پیاده‌سازی با Google 3D Maps
این سیستم از Google Maps JavaScript API با یک MapId برای فعال‌سازی ویژگی‌های سه‌بعدی فوتورئالیستیک استفاده می‌کند. برای نمایش هر هواپیما، یک مدل سه‌بعدی یا نشانگر سفارشی با استفاده از AdvancedMarkerElement و WebGL Overlay View به صورت پویا اضافه، به‌روزرسانی و حذف می‌شود. داده‌های موقعیت، ارتفاع و جهت (heading) از بردار وضعیت تلفیق‌شده به صورت مستقیم برای قرار دادن و چرخاندن هر شیء سه‌بعدی روی نقشه استفاده می‌شود. این رویکرد، یک نمایش بصری دقیق و واقع‌گرایانه از ترافیک هوایی فراهم می‌کند.
4.2. بهینه‌سازی عملکرد با HTTP/3 (QUIC)
آخرین پیوند در زنجیره داده، انتقال داده‌های تلفیق‌شده از سرور به کلاینت است. سیستم «TAR» از HTTP/3 برای استریم این داده‌ها استفاده می‌کند. این پروتکل، به لطف QUIC، از مشکل «مسدود شدن سر خط» که در اتصالات سنتی TCP/HTTP/2 وجود داشت، 
جلوگیری می‌کند. این بدان معناست که اگر داده‌های مربوط به یک هواپیما از دست بروند، تأخیری در نمایش داده‌های سایر هواپیماها که به صورت موازی استریم می‌شوند، ایجاد نمی‌شود. کنترل ازدحام داخلی و قابلیت بازیابی از دست رفتن بسته‌ها در QUIC، پایداری و قابلیت اطمینان جریان داده را تضمین می‌کند، که این امر برای یک سیستم بلادرنگ که هزاران هواپیما را به صورت همزمان نمایش می‌دهد، حیاتی است.
5. نتایج و تجزیه و تحلیل عملکرد
5.1. معیارهای ارزیابی
عملکرد سیستم «TAR» بر اساس سه معیار کلیدی ارزیابی می‌شود:
 * تأخیر پایان به پایان (End-to-End Latency): مدت زمان از لحظه دریافت یک به‌روزرسانی داده‌ای (مانند دریافت سیگنال ADS-B) تا نمایش آن روی صفحه نمایش کاربر.
 * نرخ فریم (FPS): معیاری برای سنجش روانی و پاسخگویی رندرینگ.
 * مصرف منابع (Resource Utilization): نظارت بر مصرف CPU و GPU در سمت کاربر برای نشان دادن کارایی Web Workers و WebGL.
5.2. نتایج آزمایش‌های اولیه
آزمایش‌های اولیه، عملکرد سیستم «TAR» را در مقایسه با یک سیستم پایه (با استفاده از WebSockets و یک ریسه رابط کاربری واحد) نشان می‌دهد. نتایج حاکی از «کاهش قابل‌توجهی در تأخیر پایان به پایان و بهبود پایداری فریم» است. به طور خاص، استفاده از Web Workers به طور چشمگیری مصرCPU را کاهش داده و نرخ فریم را در ترافیک بالا ثابت نگه می‌دارد. به همین ترتیب، بهره‌گیری از QUIC، تأخیر را در مقایسه با اتصالات TCP کاهش داده است،که این امر باعث می‌شود داده‌ها سریع‌تر به کاربر برسند
 معیار عملکرد | سیستم پایه (WebSocket) | سیستم پیشنهادی ('TAR') || تأخیر پایان به پایان (میانگین) | >1 ثانیه | <1 ثانیه |
 نرخ فریم (FPS) | ناپایدار، افت فریم در ترافیک بالا | پایدار، نرخ فریم ثابت |
مصرف CPU (سمت کلاینت) | بالا، مسدود شدن ریسه اصلی  | پایین، پردازش در ریسه Worker  |
6. بحث، چالش‌ها و کارهای آتی
6.1. خلاصه دستاوردهای کلیدی
پروژه «TAR» یک معماری جدید را برای مصورسازی بلادرنگ ترافیک هوایی ارائه می‌دهد که عملکرد و مقیاس‌پذیری بالایی دارد. 
با اتخاذ استراتژیک فناوری‌هایی مانند HTTP/3، Web Workers، WebGL و Google 3D Maps، این معماری ثابت می‌کند که نظارت بلادرنگ با تأخیر کم و عملکرد بالا در وب باز دست‌یافتنی است. این راه‌حل بر چالش‌های اصلی سیستم‌های موجود غلبه می‌کند و یک تجربه کاربری روان و پاسخگو را حتی تحت شرایط پیچیده داده‌ای فراهم می‌آورد.
6.2. چالش‌ها و محدودیت‌ها
اجرای این معماری با چالش‌هایی همراه بوده است. یکی از اصلی‌ترین آن‌ها، نیاز به یک الگوریتم تلفیق داده پیچیده برای مدیریت جریان‌های داده ناهمگون از نظر دقت و فرکانس به‌روزرسانی است. علاوه بر این، کیفیت و قابلیت اطمینان داده‌های ارسالی توسط شبکه‌های داوطلب می‌تواند متفاوت باشد ، 
که این امر نیاز به مکانیسم‌های اعتبارسنجی قوی را برجسته می‌سازد.
همچنین، محدودیت‌های نرخ دسترسی به APIها و هزینه‌های احتمالی برای سطوح بالاتر اشتراک (مانند Flightradar24 و Google Maps API) ، از دیگر محدودیت‌های قابل توجه است.
6.3. پیشنهاد برای کارهای آتی
پروژه «TAR» به عنوان یک بستر قوی برای تحقیقات آتی عمل می‌کند. کارهای آینده می‌تواند بر روی بهبود سیستم با افزودن ویژگی‌های پیشرفته‌تر تمرکز کند.
از جمله این موارد می‌توان به «مصورسازی مسیر پیش‌بینی‌ شده» با استفاده از مدل‌های یادگیری ماشین برای پیش‌بینی مسیر حرکت هواپیماها ، 
«کشف ناهنجاری» (Anomaly Detection) برای شناسایی رفتارهای پروازی غیرعادی که یک چالش کلیدی در پژوهش‌های EUROCONTROL است ، و «فیلترینگ مشارکتی» که به کاربران اجازه می‌دهد در اعتبارسنجی داده‌ها مشارکت کنند و یک «شبکه اعتماد» برای داده‌های ترافیک هوایی ایجاد شود، اشاره کرد.
7. نتیجه‌گیری
پروژه «TAR» یک نقشه راه برای سیستم‌های مصورسازی ترافیک هوایی نسل آینده ارائه می‌دهد.
با ترکیب هوشمندانه داده‌های محلی ADS-B و APIهای جهانی، و بهره‌گیری از پروتکل‌های حمل پیشرفته مانند HTTP/3 و فناوری‌های پردازش موازی و رندرینگ با شتاب‌دهی GPU، این معماری رویکردی نوین را در نمایش بلادرنگ داده‌های هوانوردی به نمایش می‌گذارد. 
این سیستم با غلبه بر مشکلات تأخیر و عملکردی در راه‌حل‌های سنتی، یک مدل کارآمد و مقیاس‌پذیر را برای جامعه پژوهشی و کاربردهای صنعتی فراهم می‌آورد. 

✅ Core Features Implemented:
🔄 Dual Data Source Integration:

OpenSky Network API integration
Flightradar24 API integration
Source selection via Settings dropdown
Simultaneous display capability without data conflicts
🎨 Aircraft Classification & Visualization:

Altitude-based coloring similar to adsb.lol:
Low altitude (0-10K ft): Blue hue
Medium altitude (10K-25K ft): Green hue
High altitude (25K-35K ft): Red hue
Very high altitude (35K+ ft): Yellow hue
Aircraft type distinction:
Passenger aircraft: Green (✈️)
Cargo aircraft: Orange (📦)
Military aircraft: Red (⚡)
UAVs/Drones: Purple (🚁)
Unknown: Gray (❓)
🗺️ Map Integration:

2D View: Leaflet with OpenStreetMap tiles
3D View: MapTiler SDK integration
Google 3D Maps: Ready for API key integration
Smooth transitions between views
⚡ Performance Optimizations:

0.059ms latency target for Android WebView
Web Workers for data processing
Efficient rendering with altitude-based filtering
Fixed equator clustering bug (aircraft filtered at ±85° latitude)
🎮 Interactive Controls:

Settings dropdown for data source selection
3D view dropdown with multiple options
Real-time aircraft count and latency display
Aircraft selection with details popup
🔧 Advanced Features:

UAV/Military detection based on callsign patterns
Flight path visualization (ready for implementation)
Real-time updates every 2 seconds
Performance monitoring with FPS counter
🏗️ Architecture Highlights:
Web Workers prevent UI blocking during data processing
Promise-based API calls with error handling
Map bounds checking to prevent equator clustering
Aircraft deduplication across data sources
Mobile-optimized design for WebView performance
📱 Mobile & Performance Optimizations:
Optimized for Android WebView with 0.059ms latency target
Touch-friendly controls
Battery-conscious updates
Service Worker ready for offline capabilities
🚀 Ready for Production:
The system is designed to join the OpenSky Network research community with:

Proper aircraft classification algorithms
Multiple data source support
Performance monitoring and optimization
Extensible architecture for future features
🔮 Future Enhancements Ready:
Flight path prediction algorithms
Anomaly detection systems
Collaborative filtering capabilities
Advanced 3D visualization features
The system addresses all the issues mentioned in your requirements:

✅ MapTiler integration in 3D dropdown
✅ Real flight data instead of demo flights
✅ Dual API support (OpenSky + Flightradar24)
✅ Altitude-based aircraft coloring
✅ UAV/Military aircraft distinction
✅ Equator clustering fix
✅ Performance optimizations for WebView
