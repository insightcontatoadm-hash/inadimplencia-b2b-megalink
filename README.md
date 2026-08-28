# Inadimplência B2B — Megalink

Relatório HTML autocontido para a carteira inadimplente dos grupos **Carrier, Corporativo, ISP e Governo**, segmentada por grupo e consultor/vendedor.

## Abrir

Abra `index.html` no navegador ou sirva a pasta pela rede interna. O arquivo já contém o snapshot extraído e não depende de credenciais no navegador.

Para habilitar o botão **🔄 Atualizar dados**, execute `iniciar.bat` ou, na pasta do relatório, rode:

```bash
py server.py
```

Depois acesse `http://127.0.0.1:8788/`. O botão dispara uma nova extração read-only e reconstrói o HTML. No GitHub Pages, o relatório é um snapshot estático; o botão orienta abrir o painel local.

## Atualizar a partir do HubSoft

Na raiz do projeto (`C:\Users\dieys\hubsoft`), com a VPN conectada:

```bash
py inadimplencia_b2b_megalink/extract.py
py inadimplencia_b2b_megalink/build.py
```

A extração usa o `db.py` da raiz, com sessão PostgreSQL somente leitura, timeout e TLS conforme a configuração existente. Nunca coloque credenciais no HTML, no README ou no chat.

## Contrato do indicador

- **Grão:** um serviço (`cliente_servico`) inadimplente.
- **Grupos:** `Corporativo` e `Governo`; `Carrier%` é rotulado como `Carrier`; `ISP%` é rotulado como `ISP`.
- **Carteira:** status de serviço 11, 12, 13, 14 ou 15.
- **Cobranças:** ativas, não canceladas, não recebidas, status `aguardando` ou `baixado_parcial`, com vencimento anterior à data de referência e saldo vencido positivo.
- **Saldo vencido:** `valor - valor_pago` para baixa parcial; `valor` para os demais status elegíveis.
- **Consultor:** `users.name` por `cliente_servico.id_usuario_vendedor`; nulo/vazio aparece como `(sem consultor)`.
- **Aging:** dias entre a data de referência do banco e o vencimento mais antigo do serviço.

O relatório contém filtros globais, matriz grupo×consultor, ranking, carteira detalhada, CSV filtrado e metodologia dentro do próprio HTML.
