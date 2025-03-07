# Robocopy - Guia Completo

## O que é Robocopy?

Robocopy (Robust File Copy) é uma ferramenta de linha de comando avançada, nativa do Windows, projetada para realizar cópias de arquivos de forma robusta e eficiente. Desenvolvida inicialmente como parte do Windows Resource Kit, agora é incluída por padrão em versões modernas do Windows.

## Características Principais

- **Confiabilidade**: Capaz de lidar com interrupções de rede e reiniciar cópias
- **Performance**: Suporte a operações multithreaded para cópias mais rápidas
- **Versatilidade**: Numerosas opções para personalizar o comportamento
- **Preservação**: Mantém atributos, timestamps e permissões dos arquivos
- **Automação**: Ideal para scripts e tarefas agendadas

## Sintaxe Básica

```
robocopy <origem> <destino> [arquivos] [opções]
```

- **origem**: Diretório de origem
- **destino**: Diretório de destino
- **arquivos**: Padrões de arquivos a serem copiados (opcional, padrão é `*.*`)
- **opções**: Modificadores de comportamento (opcional)

## Opções Comuns

### Opções Estruturais

| Opção | Descrição |
|-------|-----------|
| `/S` | Copia subdiretórios (exceto vazios) |
| `/E` | Copia subdiretórios (incluindo vazios) |
| `/MIR` | Espelha diretórios (cuidado: pode excluir arquivos) |
| `/PURGE` | Remove arquivos do destino que não existem na origem |
| `/XD` | Exclui diretórios especificados |
| `/XF` | Exclui arquivos especificados |

### Opções de Performance

| Opção | Descrição |
|-------|-----------|
| `/MT[:n]` | Cópia multithreaded (n = número de threads, 1-128, padrão = 8) |
| `/Z` | Modo de reinício (para arquivos grandes) |
| `/B` | Modo de backup (ignora segurança) |
| `/J` | Cópia desbuferizada (para arquivos muito grandes) |

### Opções de Tempo

| Opção | Descrição |
|-------|-----------|
| `/R:n` | Número de tentativas em caso de falha (padrão = 1 milhão) |
| `/W:n` | Tempo de espera entre tentativas em segundos (padrão = 30) |
| `/REG` | Salva estatísticas no registro do Windows |
| `/RH:hhmm-hhmm` | Executa apenas no horário especificado |

### Opções de Saída

| Opção | Descrição |
|-------|-----------|
| `/V` | Saída detalhada |
| `/NP` | Sem progresso (não mostra porcentagem) |
| `/ETA` | Mostra tempo estimado para conclusão |
| `/LOG:arquivo` | Envia saída para um arquivo de log |
| `/TEE` | Envia saída para console e arquivo de log |

## Exemplos Práticos

### Cópia Básica

```
robocopy C:\Dados D:\Backup
```

### Cópia Rápida

```
robocopy C:\Dados D:\Backup /E /Z /MT:32
```

### Espelhamento Completo

```
robocopy C:\Dados D:\Backup /MIR /FFT /Z /XA:H /W:1 /MT:32
```

### Backup com Log

```
robocopy C:\Dados D:\Backup /MIR /Z /W:5 /R:2 /LOG:C:\logs\backup.log
```

### Copiar Apenas Arquivos Específicos

```
robocopy C:\Projetos D:\Backup *.docx *.xlsx /S
```

### Excluir Arquivos Temporários

```
robocopy C:\Projetos D:\Backup /MIR /XF *.tmp *.bak
```

## Códigos de Saída

Robocopy retorna códigos de saída que podem ser usados em scripts:

| Valor | Significado |
|-------|-------------|
| 0 | Nenhum arquivo copiado |
| 1 | Arquivos copiados com sucesso |
| 2 | Arquivos extras encontrados |
| 4 | Alguns arquivos incompatíveis |
| 8 | Falhas na cópia |
| 16 | Erro fatal |

Os valores são somados, então um código 3 significa "arquivos copiados com sucesso e arquivos extras encontrados".

## Dicas e Truques

1. **Para grandes volumes de dados**:
   - Use `/MT:32` para velocidade máxima
   - Adicione `/J` para arquivos muito grandes (>2GB)

2. **Para redes instáveis**:
   - Use `/Z` para habilitar o modo de reinício
   - Reduza `/R:5 /W:15` para tentar 5 vezes esperando 15 segundos

3. **Para backups diários**:
   - Use `/MAXAGE:1` para copiar apenas arquivos modificados no último dia
   - Adicione `/LOG+:backup.log` para acrescentar ao log existente

4. **Para sincronização bidirecional**:
   - Execute duas vezes, invertendo origem e destino
   - Use `/XO` para pular arquivos mais antigos

## Considerações Importantes

- O parâmetro `/MIR` pode excluir arquivos no destino que não existem na origem
- Para testar operações antes de executá-las, use `/L` (list only)
- Robocopy é uma ferramenta poderosa, use com cautela, especialmente com opções como `/MIR` e `/PURGE`

# Exemplo de Uso Pessoal

```
robocopy "C:\BACKUP FOTOS" "E:\FOTOS" /E /Z /MT:32 /ETA /W:3 /COPYALL
```

## Referências

Para informações completas, use o comando:

```
robocopy /?
```

ou consulte a documentação oficial da Microsoft.
