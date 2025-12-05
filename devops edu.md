# devops edu


🔤مقوله DevOps شامل مجموعه‌ای از مراحل پیوسته است:
 Plan، Code، Build، Test، Release، Deploy، Operate و Monitor.

کنترل XDR فقط محدود به CI/CD نیست. حضورش در DevOps سه سطح دارد:

    سطح ۱: Pre‑runtime (Build, Test, Release)
    سطح ۲: Admission & Deploy
    سطح ۳: Runtime & Monitor (قلب XDR)


۱) عملکرد XDR  در سطح Pre‑runtime  
(Build, Scan, Release)

وXDR در این مرحله نقش Image Security Platform را دارد:

• اسکن کامل Image قبل از Push به Registry (CVE، بدافزار، Library Attack)  
• شناسایی ضعف‌های Dockerfile (مثل اجرای با user root)  
• بررسی Base Image ها و وابستگی‌ها  
• ارتباط مستقیم با Pipeline (GitLab, GitHub, Jenkins, Azure DevOps)  
• اعمال Policy: اگر Image خلاف Rule باشد بلاک 
• امضای Image (Content Trust) و تولید Evidence برای مرحله Deploy  

خروجی این مرحله:
این Image «تأیید شده» وارد Registry می‌شود و XDR یک SBOM و Risk Score به آن اختصاص می‌دهد.  
این Score در مراحل بعدی استفاده می‌شود.


۲) عملکرد XDR در Admission & Deploy  
(در ورودی Kubernetes)

اینجا XDR در این سطح در کنار Admission Controller قرار می‌گیرد:  

• بررسی اینکه ایمیج امضا شده و در مرحله Build تأیید شده باشد  
• تطبیق Manifest با Policyهای امنیتی:  
  – privileged: false  
  – hostPath ممنوع  
  – Limitها تعریف شده  
  – Image Pull از Registry معتبر  
• جلوگیری از Deploy پادهای ناسازگار  
• ارتباط با Kubernetes API Server برای Block/Allow  
• وابسته بودن تصمیم‌گیری به همان SBOM و Risk Score  
• هماهنگی با Kyverno/Opa Gatekeeper (اگر باشند)

نتیجه:  
پادهایی که در مرحله Build «پاک» شده‌اند، فقط در صورتی وارد کلاستر می‌شوند که Manifest آن‌ها هم امن باشد.  
این همان لایه دوم XDR است.


۳) عملکرد XDR در Runtime &  Monitor  
(قلب XDR در K8s)

این‌جاست که XDRقدرت اصلی‌اش را نشان می‌دهد:

• سنسور eBPF روی Node برای نظارت Syscall  
• رفتارشناسی Runtime برای تشخیص حملات واقعی:  
  – Lateral Movement  
  – Container Escape  
  – Reverse Shell  
  – Cryptomining  
  – مشکوک شدن Process Tree  
• Drift Detection:  
  – نوشتن فایل غیرمجاز  
  – اجرای باینری ناشناخته  
  – تغییر Config در Container  
  – اتصال شبکه غیرعادی  
• تحلیل Telemetry از کل کلاستر:  
  – ترافیک شبکه  
  – Process Behavior  
  – File Integrity  
  – API Server Events  
• XDR Analytics:  
  – ارتباط‌دهی بین رویدادها (Correlation)  
  – جست‌وجوی حملات End-to-End (Kill Chain View)  
• Response خودکار:  
  – توقف پاد  
  – quarantine container  
  – block outbound connection  
  – scale down سرویس آلوده  
  – Ticket به SIEM/SOAR  

نتیجه:  
اینXDRتبدیل می‌شود به چشم سازمان روی کلاستر ؛ جایی که حمله واقعی در Runtime اتفاق می افتد

---
