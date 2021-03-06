# Boolean Search SRI

Trabalho feito para a disciplina de Organização e Recuperação da Informação: realiza a construção de um indice invertido a partir de uma lista de documentos e permite uma busca booleana no corpus usando o indice invertido.

O corpus de exemplo utilizado pode ser encontrado em [CRPC sub-corpus oral espontÉneo.zip](https://github.com/AlexandreNS/term-frequency-sri/blob/main/CRPC%20sub-corpus%20oral%20espont%C3%89neo.zip?raw=true)

**OBS.:** Já foi computado o indice invertido para esse corpus e ele se encontra em [boolean-search-sri/backend/src/resources/data/](https://github.com/AlexandreNS/boolean-search-sri/tree/main/backend/src/resources/data) com `uuid = 80fa9a7d-ee4d-4f38-b24a-f5a11465473c`

## Requisitos

- NodeJS v14.0.0
- npm 6.14.4

## Instalação e Execução

Para instalação é necessario executar o comando padrão de instalação de pacotes do NPM:

```bash
# backend
# A partir da pasta raiz execute:

cd backend
npm i
```

```bash
# frontend
# A partir da pasta raiz execute:

cd frontend
npm i
```

Para executar os projetos execute `npm run start` dentro das pastas frontend e backend.

Talvez seja necessario configurar a url do backend, ela está configurada em `frontend/src/config/api_links.js`

## Projeto de Construção

Foi utilizado para o projeto o [Express](https://expressjs.com/) para a parte do Backend e o [ReactJS](https://reactjs.org/) para o Frontend

Para refinamento dos termos e construção do indice invertido foram utilizados os seguintes procedimentos:

- [html-entities](https://www.npmjs.com/package/html-entities) --> caracteres especiais do HTML
- Regex `/(<([^>]+)>)/ig` para a remoção de tags no arquivo
- Regex `/([^A-Za-zÀ-ÖØ-öø-ÿ]+)/ig` para manter apenas caracteres de texto
- Remoção de palavras com menos de 3 caracteres ou que estão na lista das [Stopwords](https://github.com/AlexandreNS/boolean-search-sri/blob/main/backend/src/resources/stopwords.js)
- Função de Stemming do pacote [natural](https://www.npmjs.com/package/natural)
- [lodash](https://www.npmjs.com/package/lodash) --> operações de busca dos termos para construir o indice

Para a tradução da string de busca e execução da busca no indice invertido utilizou-se:

- Adição no indice do termo: `{"termo":"*","docs":[],"posix":{}}` para termos não encontrados
- Regex `/([a-zØ-öø-ÿ]+)/` para substituição dos termos para o id correspondente no indice
- [lodash](https://www.npmjs.com/package/lodash) --> busca do termo no indice e operações de interseção, união e diferença de conjuntos
- [expressionparser](https://www.npmjs.com/package/expressionparser) para analisar e interpretar as operações de **AND**, **OR**, **NOT** e **NEAR** da string de busca

## Exemplo de Execução

Após a computação dos documentos do corpus, o sistema permite a realização do download de dois arquivos em `.json`:  [corpus-index.json](https://github.com/AlexandreNS/boolean-search-sri/blob/main/backend/src/resources/data/corpus-index/80fa9a7d-ee4d-4f38-b24a-f5a11465473c-CRPC-sub-corpus-oral-espontEneo.json?raw=true) e [list-docs.json](https://github.com/AlexandreNS/boolean-search-sri/blob/main/backend/src/resources/data/corpus/80fa9a7d-ee4d-4f38-b24a-f5a11465473c-CRPC-sub-corpus-oral-espontEneo.json?raw=true).

Para o corpus [CRPC sub-corpus oral espontÉneo.zip](https://github.com/AlexandreNS/term-frequency-sri/blob/main/CRPC%20sub-corpus%20oral%20espont%C3%89neo.zip?raw=true) foi possível realizar a busca `NEAR ( sexo , 0 , feminino ) AND espanhol` como exemplificação da busca booleana e obter o seguinte resultado:

![Exemplo](https://github.com/AlexandreNS/boolean-search-sri/blob/main/exemplo.png?raw=true)

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://github.com/AlexandreNS/boolean-search-sri/blob/main/LICENSE)