# 🤖 HR AI Analytics Assistant — n8n Automation

> A Persian-language AI-powered HR assistant built with n8n, Telegram, RAG, Qdrant, AvalAI and Power BI Service.

---

## 🇮🇷 فارسی

### معرفی پروژه

مینی پروژه **HR AI Analytics Assistant** یک Automation مبتنی بر n8n است که به کاربر اجازه می‌دهد سؤالات منابع انسانی خود را به زبان طبیعی و از طریق Telegram مطرح کند و متناسب با نوع سؤال، پاسخ مبتنی بر اسناد HR یا تحلیل Power BI دریافت کند.

هدف پروژه، ایجاد یک لایه هوشمند بین کاربر و منابع اطلاعاتی سازمان است تا کاربر بدون نیاز به نوشتن Query یا جست‌وجوی دستی در اسناد و داشبوردها، پاسخ موردنیاز خود را دریافت کند.

این پروژه بر پایه‌ی یک دیتاست و اسناد HR کاملاً فرضی ساخته شده و به هیچ سازمان واقعی مرتبط نیست.

### 🏗️ معماری نهایی

دو مسیر اصلی دارد: Policy / RAG و Power BI 

**📚 Policy / RAG**

برای سؤالات مربوط به:
- سیاست‌ها و آیین‌نامه‌های منابع انسانی
- قوانین و فرآیندهای HR
- اطلاعات موجود در مستندات سازمان

سؤال کاربر به Embedding تبدیل شده و با استفاده از Qdrant در محتوای HR جست‌وجو می‌شود. Context بازیابی‌شده سپس برای تولید پاسخ به LLM ارسال می‌شود.


**📊 Power BI**

برای سؤالات داده‌محور و مدیریتی:

Natural Language Question
→ Intent / Page Detection
→ Filter Extraction
→ Measure Selection
→ Filter Normalization
→ DAX Query Generation
→ Power BI Execute Queries
→ Result Formatting
→ Final Response


همچنین در صورت درخواست کاربر، تصویر صفحه‌ی مرتبط Power BI ارسال می‌شود؛ اگر سؤال شامل فیلتر باشد، این فیلترها (برای فیلترهای پشتیبانی‌شده مانند جنسیت، وضعیت تأهل، واحد سازمانی، سطح شغلی، سطح تحصیلات، تعلق شغلی و اضافه‌کاری) همزمان روی خود تصویر اعمال می‌شود؛ در غیر این صورت (بدون فیلتر خاص)، تصویر کامل همان صفحه ارسال می‌شود.


### 🧠 LLM & RAG

برای پردازش زبان طبیعی و Intent Classification از **AvalAI با مدل GPT-4o-mini** استفاده شده است.
برای بخش RAG از Qdrant به‌عنوان Vector Database و مدل چندزبانه paraphrase-multilingual-MiniLM-L12-v2 (از طریق Hugging Face Inference API) برای تولید Embedding استفاده شده است.



### 🔎 مدیریت فیلترها

یکی از بخش‌های مهم پروژه، تبدیل ورودی‌های متنوع فارسی کاربر به مقادیر استاندارد قابل استفاده در Power BI است.

برای این کار یک **Master Dictionary / Normalization Layer** ایجاد شده که مواردی مانند جنسیت، وضعیت تأهل، واحد سازمانی، سطح شغلی، سطح تحصیلات، زمینه تحصیلات، تعلق شغلی و سایر فیلترها را Normalize می‌کند.

همچنین نام ستون‌های واقعی مدل Power BI در زمان ساخت Query رعایت می‌شود.

### 🖼️ Power BI Screenshot

در سؤالات مدیریتی که کاربر درخواست تصویر داشته باشد، Workflow علاوه بر محاسبه نتیجه، صفحه مرتبط Power BI را Export کرده و تصویر را در Telegram ارسال می‌کند.

فیلترهای پشتیبانی‌شده روی خود تصویر هم اعمال می‌شوند (جزئیات در بخش معماری بالا).

### 🗄️ PostgreSQL

در نسخه نهایی PostgreSQL به‌عنوان مسیر مستقل تحلیل داده استفاده نمی‌شود.

کاربرد آن در Automation، ثبت و نگهداری اطلاعات تعاملات و Logging Workflow است.

### 📊 Validation

Workflow با 100 سؤال متنوع در سناریوهای مختلف تست شد.



نتیجه تست نهایی:

**98 پاسخ صحیح از 100 تست — 98% Accuracy**

این تست‌ها شامل سؤالات Policy، سؤالات عددی Power BI، فیلترهای چندگانه، سؤالات رتبه‌بندی و مقایسه‌ی گروهی (مانند «کدام واحد بیشترین نرخ ترک خدمت را دارد؟»)، سؤالات مدیریتی، انتخاب صفحه، Measure Selection و درخواست Screenshot بوده‌اند.



### 🛠️ Tech Stack

- n8n
- Telegram Bot
- AvalAI / GPT-4o-mini
- Qdrant
- HuggingFace Inference API
- Power BI Service
- DAX
- PostgreSQL
- Docker

### 🔐 Security

این Repository شامل Credential، API Key، Token، Password یا اطلاعات حساس واقعی نیست.

قبل از Import کردن Workflow، Credentialهای سرویس‌ها باید در محیط n8n مقصد دوباره تنظیم شوند.

داده‌های واقعی و اطلاعات شخصی کارکنان نیز نباید در Repository عمومی قرار گیرند.

---

## 🇬🇧 English

### Project Overview

**HR AI Analytics Assistant** is an n8n-based automation that allows users to ask HR-related questions in natural language through Telegram.

Depending on the question, the workflow routes the request to either the **Policy/RAG path** or the **Power BI path**, returning a relevant textual, numerical, or visual response.

The main goal is to provide a natural-language interface between HR users and organizational knowledge sources, reducing the need for manual document search, query writing, or dashboard navigation.

This project is built on a fully fictional HR dataset and documents, and is not affiliated with any real organization.

### 🏗️ Final Architecture

The final workflow contains two main paths:

**📚 Policy / RAG**

Used for HR policy, procedure, regulation, and document-based questions.

The question is converted into an embedding and searched against HR knowledge stored in Qdrant. Retrieved context is then passed to the LLM to generate a grounded response.

**📊 Power BI**

Used for analytical and managerial questions.

Natural Language Question
→ Intent / Page Detection
→ Filter Extraction
→ Measure Selection
→ Filter Normalization
→ DAX Query Generation
→ Power BI Execute Queries
→ Result Formatting
→ Final Response

When requested, the workflow also sends a screenshot of the relevant Power BI page. If the question includes filters, they are applied directly to the exported image as well (for supported dimensions such as gender, marital status, department, job level, education level, job involvement, and overtime); otherwise, the full unfiltered page is sent.

### 🧠 LLM & RAG

The workflow uses **AvalAI with GPT-4o-mini** for natural-language processing and intent classification.

**Qdrant** is used as the vector database, while paraphrase-multilingual-MiniLM-L12-v2 (called via the Hugging Face Inference API) is used for multilingual embeddings.


### 🔎 Filter Normalization

A custom **Master Dictionary / Normalization Layer** was implemented to map different Persian user expressions to standardized Power BI filter values.

This layer handles dimensions such as gender, marital status, department, job level, education level, education field, job involvement, and other HR filters.

### 🖼️ Power BI Integration

For managerial requests requiring a visual response, the workflow exports the relevant Power BI page and sends the resulting image to Telegram, applying supported filters directly to the image where possible.

### 🗄️ PostgreSQL

In the final architecture, PostgreSQL is not used as an independent analytics path.

It is used for interaction logging and storing workflow-related interaction data.

### 📊 Validation

The final workflow was tested with **100 different questions**.

**98 correct responses out of 100 — 98% accuracy.**

Test scenarios covered policy questions, Power BI analytical questions, multiple filters, ranking and group-comparison questions (e.g. "which department has the highest attrition rate?"), managerial requests, page selection, measure selection, and dashboard screenshot requests.

### 🛠️ Tech Stack

- n8n
- Telegram Bot
- AvalAI / GPT-4o-mini
- Qdrant
- HuggingFace Inference API
- Power BI Service
- DAX
- PostgreSQL
- Docker

### 🔐 Security

No real credentials, API keys, tokens, passwords, or sensitive employee information should be included in this repository.

Credentials must be configured separately in the target n8n environment.

Real employee datasets and personally identifiable information should not be committed to a public repository.

---

