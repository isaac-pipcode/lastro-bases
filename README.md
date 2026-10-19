# lastro-bases

Bases abertas do **Lastro Eleitoral** em CSV, servidas ao site
([lastroeleitoral.vercel.app](https://lastroeleitoral.vercel.app)) em vez de ficarem dentro do
deployment da Vercel.

## Por quê

O site publica 3 vezes por dia. As 39 bases somam **166 MB**, e iam inteiras em cada deployment —
o Deployment Storage do plano Hobby (10 GB) estourou em 8 dias e a Vercel **bloqueou novos
deploys** (18/10/2026). Hospedando as bases aqui, cada deployment cai para ~128 MB.

## Como o site consome

`cruzamento.html` monta o nome do arquivo e busca por URL absoluta:

```js
const BASE_BASES = "https://raw.githubusercontent.com/isaac-pipcode/lastro-bases/main/";
const r = await fetch(BASE_BASES + arq);   // arq = "base_secao_2026_SP.csv", etc.
```

`raw.githubusercontent.com` responde com `Access-Control-Allow-Origin: *`, então não precisa
configurar nada. O arquivo é servido como `text/plain`; o `cruzamento.html` já faz `r.text()` e
passa pelo parser de CSV, então o tipo não atrapalha.

## Contrato

- **Os nomes dos arquivos são o contrato** com o site e não mudam: `base_secao_2026_<UF>.csv`,
  `base_zona_2026.csv`, `base_bairro_2026.csv`, `base_municipio_secao_2026.csv`,
  `base_candidato_zona_2026.csv`, `base_abst_perfil_2022.csv`.
- As colunas também não mudam (ver `DICIONARIO.md` no repositório principal).
- **Fonte da verdade:** `isaac-pipcode/mapa-eleitoral-2026`. Este repositório é uma cópia
  publicada; quando as bases forem atualizadas lá, são recopiadas para cá.

## Dados

Origem: TSE (bases públicas de seções, perfil do eleitorado 2022, candidatos por zona). Nada aqui
é sigiloso — o site já oferecia esses arquivos para download.

Atualizado em 19/10/2026.
