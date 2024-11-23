# Tradutor-de-artigos-tecnicos-com-open-ai
Resposta ao desafio DIO - Tradutor de artigos tecnicos com open ai

Criar um tradutor de artigos técnicos com Azure OpenAI usando Python envolve a integração da API da OpenAI do Azure com um fluxo de tradução. Abaixo está o script que utiliza a API do Azure OpenAI para traduzir texto técnico.  

### Pré-requisitos

1. **Conta no Azure** com o serviço OpenAI configurado.  
2. **API Key** e **endpoint** do serviço Azure OpenAI.  
3. **Python 3+** instalado no sistema.  
4. Instalar a biblioteca `requests` (se ainda não estiver instalada):  
   ```bash
   pip install requests
   ```

### Código Python

```python
import requests

# Configurações do Azure OpenAI
AZURE_OPENAI_API_KEY = "SUA_CHAVE_API"
AZURE_OPENAI_ENDPOINT = "SEU_ENDPOINT"
AZURE_OPENAI_DEPLOYMENT = "SEU_NOME_DO_DEPLOYMENT"  # Nome do modelo configurado no Azure

def traduzir_texto(texto, idioma_destino="en"):
    """
    Traduz um texto usando o Azure OpenAI.
    
    :param texto: Texto a ser traduzido.
    :param idioma_destino: Idioma para tradução (ex.: 'en' para inglês, 'pt' para português).
    :return: Texto traduzido.
    """
    prompt = f"Traduza o seguinte texto técnico para {idioma_destino}:\n\n{texto}"

    headers = {
        "Content-Type": "application/json",
        "api-key": AZURE_OPENAI_API_KEY,
    }

    payload = {
        "prompt": prompt,
        "max_tokens": 500,  # Ajuste conforme necessário
        "temperature": 0.3,  # Menor temperatura para maior precisão técnica
    }

    response = requests.post(
        f"{AZURE_OPENAI_ENDPOINT}/openai/deployments/{AZURE_OPENAI_DEPLOYMENT}/completions",
        headers=headers,
        json=payload,
    )

    if response.status_code == 200:
        result = response.json()
        return result["choices"][0]["text"].strip()
    else:
        print(f"Erro na API: {response.status_code} - {response.text}")
        return None


if __name__ == "__main__":
    texto_tecnico = """
    Redes neurais convolucionais são amplamente utilizadas em visão computacional. 
    Essas arquiteturas são compostas por camadas convolucionais, pooling e completamente conectadas.
    """
    idioma = "en"  # Exemplo: traduzir para inglês
    traducao = traduzir_texto(texto_tecnico, idioma_destino=idioma)

    if traducao:
        print("Texto Traduzido:")
        print(traducao)
```

### Explicação do Código

1. **Configuração da API**: Inclua a chave de API, o endpoint e o nome do modelo que você configurou no Azure OpenAI.  

2. **Prompt**: O prompt é essencial para instruir o modelo a realizar uma tradução técnica, mantendo precisão e contexto.  

3. **Parâmetros**:
   - `max_tokens`: Define o comprimento máximo da resposta.
   - `temperature`: Mantida baixa para resultados mais previsíveis e técnicos.  

4. **Resultado**: O script utiliza a API da Azure OpenAI para gerar a tradução e retorna o texto traduzido.  

### Como Ajustar

- **Idioma de destino**: Modifique `idioma_destino` conforme necessário (ex.: `'es'` para espanhol, `'fr'` para francês).  
- **Tamanho do texto**: Se for traduzir artigos muito longos, divida o texto em partes menores e envie para a API em lotes.  

Se precisar de mais detalhes, é só perguntar! 😊
