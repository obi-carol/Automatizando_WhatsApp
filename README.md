# Automatizando_WhatsApp
Olá, segue um novo trabalho de envio de mensagens automática no WhatsApp.

from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
import time
import pyperclip

# Crie o serviço para gerenciar o ChromeDriver
service = Service(ChromeDriverManager().install())

# Crie o navegador da web usando o serviço
nav = webdriver.Chrome(service=service)

# Abrir o WhatsApp Web
nav.get("https://web.whatsapp.com/")
input("Faça login no WhatsApp Web e pressione Enter para continuar...")

# Lista de contatos
contatos = ["Contato1", "Contato2", "Contato3", "Contato4", "Contato5", "Contato6"]  # Adicione seus contatos aqui

# Mensagem a ser encaminhada
mensagem = "Sua mensagem aqui"

# Loop para encaminhar mensagens em grupos de 5
for i in range(0, len(contatos), 5):
    grupo = contatos[i:i+5]

    for contato in grupo:
        # Abre a conversa com o contato
        nav.get(f"https://web.whatsapp.com/send?phone={contato}")
        time.sleep(5)  # Aguarde um tempo para carregar a página

        # Cole a mensagem da área de transferência e envie
        pyperclip.copy(mensagem)
        nav.find_element_by_xpath('//div[@contenteditable="true"]').send_keys(Keys.CONTROL + 'v')
        nav.find_element_by_xpath('//span[@data-icon="send"]').click()

# Encerrar o navegador
nav.quit()
