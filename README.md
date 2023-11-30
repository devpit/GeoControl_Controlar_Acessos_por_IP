# Controle de Acesso ao Site

Este script PHP foi desenvolvido para restringir o acesso ao seu site apenas a usuários localizados no Brasil (BR). Ele utiliza o endereço IP do usuário para determinar a localização geográfica e redireciona aqueles fora do Brasil para "https://google.com".

## Como Funciona

1. **Sessão e IP:** Inicia uma sessão para armazenar o endereço IP do usuário.

    Adicione este código em um arquivo chamado `getconfig.php`
    ```php
    <?php
    session_start();
    $ip = $_SERVER['REMOTE_ADDR'];
    $_SESSION['ip'] = $ip;
    echo $ip;
    ?>
    ```

2. **Configuração Frontend:** Um formulário exibe o endereço IP e permite atualizar as informações.

   ```html
   <body onload="getConfig()">
     <form style="display: none">
       <input type="hidden" name="ip" id="ip" />
       <button type="button" onclick="getConfig()"></button>
     </form>
   </body>
   ```

3. **Função getConfig():**

   ```javascript
   function getConfig() {
     var xhr = new XMLHttpRequest();
     xhr.onreadystatechange = function () {
       if (this.readyState === 4 && this.status === 200) {
         var ip = this.responseText;
         document.getElementById("ip").value = ip;

         consultarPais(ip);
       }
     };
     xhr.open("GET", "getconfig.php", true);
     xhr.send();
   }

   function consultarPais(ip) {
     var url = "https://ipinfo.io/" + ip + "/json";

     var xhrPais = new XMLHttpRequest();
     xhrPais.onreadystatechange = function () {
       if (this.readyState === 4 && this.status === 200) {
         var data = JSON.parse(this.responseText);
         var country = data.country;

         if (country !== "BR") {
           window.location.href = "https://google.com";
         }
       }
     };
     xhrPais.open("GET", url, true);
     xhrPais.send();
   }
   ```

4. **Geolocalização por IP:** Consulta a API [ipinfo.io](https://ipinfo.io/) para obter o país do usuário.

   ```javascript
   function consultarPais(ip) {
     var url = "https://ipinfo.io/" + ip + "/json";
     // ...
   }
   ```

5. **Controle de Acesso:** Redireciona para "https://google.com" se o país não for o Brasil.

   ```javascript
   // Ajuste conforme suas necessidades...
   if (country !== "BR") {
     window.location.href = "https://google.com";
   }
   ```

## Como Utilizar

1. **Integração:** Inclua o código PHP nas páginas onde deseja controlar o acesso.

2. **Configuração Frontend:** Personalize o formulário conforme necessário.

3. **Nota:** Certifique-se de que a extensão `cURL` está habilitada no PHP.

4. **Personalização:** Adapte conforme necessário e parabéns pelo primeiro ano deste script!

Certifique-se de ajustar para atender às suas necessidades e cumprir as políticas da API [ipinfo.io](https://ipinfo.io/). 3. **Aprimoramentos:** Você pode personalizar este script conforme necessário para se adequar aos requisitos específicos do seu projeto. Isso pode incluir aprimoramentos na interface do usuário, como mensagens de aviso ou páginas de destino diferentes para redirecionamento.
