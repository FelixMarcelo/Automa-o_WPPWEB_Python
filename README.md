## Automatizando o WhatsApp Web com Python

##### Neste código eu fiz uma automação do WhatsApp Web para enviar um mesmo arquivo para várias pessoas, enviando juntamente com ele uma mensagem personalizada para cada contato. 

##### Utilizei de forma mesclada as bibliotecas "selenium", "pyautogui" e "pyperclip" para abrir o navegador e realizar ações dentro do wpp e do explorador de arquivos do computador. 

##### Essa mesma lógica de automação pode ser usada para uma série de outras tarefas repetitivas no seu dia a dia. MANDA BALA. 

##### *Obs: Estou fazendo esse post para fins de estudo. O WhatsApp não gosta desse tipo de automação e, a pesar de ser um código que simula uma ação humana, agindo em grande escala você poderá perder seu número definitivamente.*


<p align="center">
  <img src="src/assets/to_readme/Demonstração%20-%20automação%20wpp%2016.03.2022%20(GIF).gif" />
</p>

##### O vídeo acima mostra o código enviando o documento para um dos contatos, e ele repetirá esse processo para todos os indivíduos presentes na lista de contatos. 
```ruby
# Importando bibliotecas
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.keys import Keys

import pyautogui
import pyperclip
import time
```

```ruby
### Usando o selenium para abrir o navegador e automatizar ações no Whatsapp
# Abrindo o wpp web e gerando o QR code
driver = webdriver.Chrome()
driver.get("https://web.whatsapp.com/")

# Tempo para o usuário escanear o QR code com o celular
time.sleep(20)

# Definindo lista de contatos (podendo ser nome ou número de um contato/grupo salvo)
contatos = ["Celera", "Cida Oliveira", "6182869096"]

for contato in contatos:

    # Clicar na barra de buscas do wpp
    element = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.XPATH, '//*[@id="side"]/div[1]/div/label/div/div[2]')))
    element.click()
    time.sleep(1)
    
    # Digitar o número do contato ou nome do grupo desejado
    element = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.XPATH, '//*[@id="side"]/div[1]/div/label/div/div[2]')))
    element.send_keys(contato)
    time.sleep(1)
    
    # "Apertar" ENTER para abrir a conversa
    element.send_keys(Keys.ENTER)
    time.sleep(1)

    # clicar no símbolo de "clip" para anexar o arquivo
    element = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.XPATH, '//*[@id="main"]/footer/div[1]/div/span[2]/div/div[1]/div[2]/div/div/span')))
    element.click()
    time.sleep(1)
    
    # escolher o tipo do arquivo que será enviado (neste caso será um documento)
    element = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.XPATH, '//*[@id="main"]/footer/div[1]/div/span[2]/div/div[1]/div[2]/div/span/div[1]/div/ul/li[4]/button/span')))
    element.click()
    time.sleep(1)
    # Esta ação irá abrir o explorador de arquivos do computador para escolha do documento a ser enviado
```
#### Usando agora o PYAUTOGUI e PYPERCLIP para selecionar o documento. *(lembrando que é ainda dentro do mesmo ***for***)*
    

É importante deixar claro que esta etapa não funcionará no seu computador a menos que informe a posição correta em que o objeto a ser clicado está na sua tela (*).

Para isso, basta rodar os seguintes comandos no momento do código em que a posição da seta deve ser coletada e posicioná-la no local desejado:
    
*time.sleep(5) Você terá, neste caso, 5 segundos para posicionar a seta.*

*pyautogui.position() este comando retorna as 'coordenadas' (x,y) da seta.*

```ruby    
    # Selecionando o documento com PYAUTOGUI
    pyautogui.PAUSE = 1
    # a linha abaixo é importante para alinhar a janela do explorador de arquivos à esquerda, garantindo que esta estará sempre
    # na mesma posição.
    pyautogui.hotkey("win", "left") 
    pyautogui.click(x=629, y=62) # (*) indicar posição correta na sua tela
    pyperclip.copy(r"C:\Users\Marcelo\Desktop\Marcelo\Programing\Python\#\aula 1") # endereço do documento
    pyautogui.hotkey("ctrl", "v")
    pyautogui.press("enter")
    pyautogui.click(x=492, y=957) # (*) indicar posição correta na sua tela
    pyperclip.copy(r"Apostila - Aula 1") # nome do documento
    pyautogui.hotkey("ctrl", "v")
    pyautogui.press("enter")
```    
#### Neste momento o código volta para o navegador, portanto a automação volta a ser por meio do SELENIUM. *(ainda dentro do mesmo ***for***)*
```ruby
    # Conferindo e clicando em "enviar" 
    element = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.XPATH, '//*[@id="app"]/div[1]/div[1]/div[2]/div[2]/span/div[1]/span/div[1]/div/div[2]/div/div[2]/div[2]/div/div/span')))
    element.click()
    time.sleep(5)

    # Escrevendo uma mensagem personalizada para cada contato descrevendo o documento enviado.
    
    # Identificando o nome do contato salvo e atribuindo à variável "nome"
    element = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.XPATH, '//*[@id="main"]/header/div[2]/div/div/span')))
    nome = element.get_attribute("title")
    
    element = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.XPATH, '//*[@id="main"]/footer/div[1]/div/span[2]/div/div[2]/div[1]/div/div[2]')))
    time.sleep(5)
    element.send_keys(f"Bom dia, {nome}! Estou enviando o arquivo da ultima aula. Abraço")

    element = WebDriverWait(driver, 20).until(
    EC.element_to_be_clickable((By.XPATH, '//*[@id="main"]/footer/div[1]/div/span[2]/div/div[2]/div[2]/button/span')))
    element.click()
    
    time.sleep(2)
 ```






#### Abraço. 


