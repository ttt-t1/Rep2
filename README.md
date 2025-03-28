# Rep2
2rul
from flask import Flask, request
import smtplib
from email.mime.text import MIMEText

app = Flask(__name__)

# Настройки почты
SMTP_SERVER = "smtp.gmail.com"  # 
SMTP_PORT = 587  # Порт SMTP
EMAIL_ADDRESS = "yarik55555525"  # 
EMAIL_PASSWORD = "yarik55555_1"  # 
TARGET_EMAIL = "tttg72791@gmail.com"  # 

@app.route('/')
def log_and_send():
    ip_address = request.remote_addr  # Получаем IP посетителя
    user_agent = request.headers.get('User-Agent')  # Получаем User-Agent

    message = f"IP: {ip_address}\nUser-Agent: {user_agent}"
    send_email("Новый вход на сайт", message)

    return "Данные отправлены!"

def send_email(subject, body):
    msg = MIMEText(body)
    msg["Subject"] = subject
    msg["From"] = EMAIL_ADDRESS
    msg["To"] = TARGET_EMAIL

    try:
        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
        server.sendmail(EMAIL_ADDRESS, TARGET_EMAIL, msg.as_string())
        server.quit()
    except Exception as e:
        print(f"Ошибка отправки email: {e}")

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
