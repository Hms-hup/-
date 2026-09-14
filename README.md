# -
تكتب معلوماتك لترسلها لي في الواتس اب ويب
import streamlit as st
import urllib.parse

# إعدادات الصفحة والعنوان
st.set_page_config(page_title="إرسال معلومات المتحدث", page_icon="📋", layout="centered")

# جعل الواجهة تدعم المحاذاة من اليمين إلى اليسار (RTL) للغة العربية
st.markdown("""
    <style>
    .stApp { direction: rtl; text-align: right; }
    div[data-testid="stForm"] { border-radius: 12px; }
    </style>
""", unsafe_allow_html=True)

# الرقم الثابت بالصيغة الدولية بدون علامة +
WHATSAPP_NUMBER = "966540909634"

st.title("🚀 إرسال حساب المتحدث للواتساب")
st.write("أدخل البيانات أدناه ليتم توليد نص البطاقة وتوجيهك للواتساب فوراً.")

# نموذج إدخال البيانات
with st.form("user_form", clear_on_submit=False):
    name = st.text_input("👤 اسم المستخدم")
    age_str = st.text_input("🎂 العمر (أرقام)")
    job = st.text_input("💼 المهنة / الوظيفة")

    submit_button = st.form_submit_button("إرسال عبر الواتساب")

if submit_button:
    if name and age_str and job:
        try:
            age = int(age_str)
            status = "شباب" if age < 40 else "كاهل"

            # تجهيز نص الرسالة وتنسيقه للواتساب
            whatsapp_message = (
                f"📋 *بطاقة معلومات المستخدم*\n\n"
                f"👤 *الاسم الكامل:* {name}\n"
                f"🎂 *العمر الحالي:* {age} سنة ({status})\n"
                f"💼 *المهنة / الوظيفة:* {job}"
            )

            # ترميز النص ليكون متوافقاً مع الروابط (URL Encoding)
            encoded_message = urllib.parse.quote(whatsapp_message)

            # إنشاء رابط الواتساب المباشر
            whatsapp_url = f"https://wa.me{WHATSAPP_NUMBER}?text={encoded_message}"

            st.success("✅ تم تجهيز البطاقة بنجاح!")

            # زر مخفي ذكي يفتح الرابط في نافذة جديدة (يعمل على كافة الهواتف)
            link_html = f'<a href="{whatsapp_url}" target="_blank" style="text-decoration: none;"><div style="background-color: #107C41; color: white; padding: 10px 20px; text-align: center; border-radius: 8px; font-weight: bold; cursor: pointer;">اضغط هنا لفتح الواتساب وإرسال البطاقة</div></a>'
            st.markdown(link_html, unsafe_allow_html=True)

        except ValueError:
            st.error("⚠️ خطأ: يرجى كتابة العمر بالأرقام فقط!")
    else:
        st.warning("⚠️ تنبيه: يرجى تعبئة جميع الحقول المطلوبة.")
