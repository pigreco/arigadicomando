# Gestione formati

Il **formato nativo** di Miller è il [**`DKVP`**](#dkvp-il-formato-nativo) ("*Delimited Key-Value Pairs*"), ovvero delle coppie chiave-valore, separate da virgola (la `,` è il separatore di default).<br>
A seguire un esempio.

!!! info "Esempio del formato nativo. Notare che i record non hanno lo stesso numero di campi"

    ```
    nome=andy,dataNascita=1973-05-08,altezza=176,peso=86.5,comuneNascita=Roma
    nome=chiara,dataNascita=1993-12-13,altezza=162,peso=58.3,comuneNascita=Milano
    nome=guido,altezza=196,peso=90.4,comuneNascita=Roma
    nome=sara,dataNascita=2000-02-22,altezza=166,peso=70.4,comuneNascita=Roma
    nome=giulia,dataNascita=1997-08-13,altezza=169,peso=68.3
    ```

Spesso si pensa ai dati come "rettangolari": se sono previsti 10 campi, ogni record sarà composto da 10 valori.<br>
Ma non è sempre così, come nel caso del formato [`JSON`](#json) o appunto di quello nativo di Miller, in cui ogni record non ha necessariamente lo stesso numero di campi degli altri.

Miller di *default* gestisce quindi l'[**eterogeneità dei record**](eterogeneita_record.md).

## Conversione di formato

Miller legge e scrive [diversi formati di testo strutturato](formati.md#elenco-formati). Per impostare quello di *input* e di *output* è necessario utilizzare uno dei [flag dedicati](flag.md#formati-file) e una delle modalità per farlo.

Ad esempio per convertire un file da `CSV` a `TSV`, si può usare questo comando:

```bash
mlr --icsv --otsv cat input.csv>output.csv
```

Nel dettaglio:

- `--icsv` per impostare il formato di *<b>i</b>nput*;
- `--ocsv` per impostare il formato di *<b>o</b>utput*;
- `cat` è uno dei [verbi](verbi.md) di Miller, quello di base, che passa i dati senza alcuna trasformazione dall'*input* all'*output*.

!!! attention "Nota  bene"

    In un comando Miller, è sempre necessario inserire almeno uno dei suoi verbi. Qui è `cat`.

C'è anche la **versione "breve"** dello stesso comando, in cui `--icsv --otsv`, diventa `--c2t` (ovvero da `CSV` a `TSV`, che in inglese è "`CSV` TO `TSV`"):


```bash
mlr --c2t cat input.csv>output.csv
```

Qui a seguire i [*flag*](flag.md) di base, per passare da uno dei possibili formati di *input* a uno di quelli di *output*.


| IN/OUT   | **CSV** | **TSV** | **JSON** | **DKVP** | **NIDX** | **XTAB** | **PPRINT** | **Markdown** |
|------------|---------|---------|----------|----------|----------|----------|------------|--------------|
| **CSV**    |         | `--c2t` | `--c2j`  | `--c2d`  | `--c2n`  | `--c2x`  | `--c2p`    | `--c2m`      |
| **TSV**    | `--t2c` |         | `--t2j`  | `--t2d`  | `--t2n`  | `--t2x`  | `--t2p`    | `--t2m`      |
| **JSON**   | `--j2c` | `--j2t` |          | `--j2d`  | `--j2n`  | `--j2x`  | `--j2p`    | `--j2m`      |
| **DKVP**   | `--d2c` | `--d2t` | `--d2j`  |          | `--d2n`  | `--d2x`  | `--d2p`    | `--d2m`      |
| **NIDX**   | `--n2c` | `--n2t` | `--n2j`  | `--n2d`  |          | `--n2x`  | `--n2p`    | `--n2m`      |
| **XTAB**   | `--x2c` | `--x2t` | `--x2j`  | `--x2d`  | `--x2n`  |          | `--x2p`    | `--x2m`      |
| **PPRINT** | `--p2c` | `--p2t` | `--p2j`  | `--p2d`  | `--p2n`  | `--p2x`  |            | `--p2m`      |


## Elenco formati

Il file di riferimento di *input*, usato per produrre i vari formati di output è [`base_category.csv`](risorse/base_category.csv).<br>
Per ognuno di questi, è stato inserito il comando per generarlo a partire dal `CSV` di *input*.

### CSV

!!! comando "mlr --csv cat base_category.csv"

    ```
    andy,1973-05-08,176,86.5,Roma
    chiara,1993-12-13,162,58.3,Milano
    guido,2001-01-22,196,90.4,Roma
    sara,2000-02-22,166,70.4,Roma
    giulia,1997-08-13,169,68.3,Milano
    ```

### DKVP (il formato nativo)


!!! comando "mlr --c2d cat base_category.csv"

    ```
    nome=andy,dataNascita=1973-05-08,altezza=176,peso=86.5,comuneNascita=Roma
    nome=chiara,dataNascita=1993-12-13,altezza=162,peso=58.3,comuneNascita=Milano
    nome=guido,dataNascita=2001-01-22,altezza=196,peso=90.4,comuneNascita=Roma
    nome=sara,dataNascita=2000-02-22,altezza=166,peso=70.4,comuneNascita=Roma
    nome=giulia,dataNascita=1997-08-13,altezza=169,peso=68.3,comuneNascita=Milano
    ```

A seguire, lo stesso input in altri dei formati supportati da Miller.


### TSV

!!! comando "mlr --c2t cat base_category.csv"

    ```
    nome    dataNascita     altezza peso    comuneNascita
    andy    1973-05-08      176     86.5    Roma
    chiara  1993-12-13      162     58.3    Milano
    guido   2001-01-22      196     90.4    Roma
    sara    2000-02-22      166     70.4    Roma
    giulia  1997-08-13      169     68.3    Milano
    ```

### NIDX: Index-numbered

!!! comando "mlr --c2n cat base_category.csv"

    ```
    andy 1973-05-08 176 86.5 Roma
    chiara 1993-12-13 162 58.3 Milano
    guido 2001-01-22 196 90.4 Roma
    sara 2000-02-22 166 70.4 Roma
    giulia 1997-08-13 169 68.3 Milano
    ```

### JSON

!!! warning

    Il JSON di output di default di Miller 5 non è propriamente un JSON.

In **Miller 5** (versione attuale, che a breve sarà superata dalla 6), l'output di *default*  è il [JSON Lines](https://jsonlines.org/). La scelta deriva dal fatto che è un formato molto più comodo per l'elaborazione con strumenti di *parsing* di testo e di versionamento; perché in questo formato ogni linea è un JSON valido, e l'elaborazione per linea è molto più comoda e tipica per i *client*.

È quindi come sotto:

!!! comando "mlr --c2j cat base_category.csv"

    ```json
    {"nome": "andy", "dataNascita": "1973-05-08", "altezza": 176, "peso": 86.5, "comuneNascita": "Roma"}
    {"nome": "chiara", "dataNascita": "1993-12-13", "altezza": 162, "peso": 58.3, "comuneNascita": "Milano"}
    {"nome": "guido", "dataNascita": "2001-01-22", "altezza": 196, "peso": 90.4, "comuneNascita": "Roma"}
    {"nome": "sara", "dataNascita": "2000-02-22", "altezza": 166, "peso": 70.4, "comuneNascita": "Roma"}
    {"nome": "giulia", "dataNascita": "1997-08-13", "altezza": 169, "peso": 68.3, "comuneNascita": "Milano"}
    ```

Se si vuole un "vero" JSON bisogna aggiungere il [*flag*](flag.md#json) `--jlistwrap`:

!!! comando "mlr --c2j --jlistwrap cat base_category.csv"

    ```json
    [
    { "nome": "andy", "dataNascita": "1973-05-08", "altezza": 176, "peso": 86.5, "comuneNascita": "Roma" }
    ,{ "nome": "chiara", "dataNascita": "1993-12-13", "altezza": 162, "peso": 58.3, "comuneNascita": "Milano" }
    ,{ "nome": "guido", "dataNascita": "2001-01-22", "altezza": 196, "peso": 90.4, "comuneNascita": "Roma" }
    ,{ "nome": "sara", "dataNascita": "2000-02-22", "altezza": 166, "peso": 70.4, "comuneNascita": "Roma" }
    ,{ "nome": "giulia", "dataNascita": "1997-08-13", "altezza": 169, "peso": 68.3, "comuneNascita": "Milano" }
    ]
    ```

In **Miller 6** (prossimo al rilascio) l'*output* di *default* è invece un `JSON`:

!!! comando "mlr --c2j cat base_category.csv"

    ```json
    [
      {
        "nome": "andy",
        "dataNascita": "1973-05-08",
        "altezza": 176,
        "peso": 86.5,
        "comuneNascita": "Roma"
      },
      {
        "nome": "chiara",
        "dataNascita": "1993-12-13",
        "altezza": 162,
        "peso": 58.3,
        "comuneNascita": "Milano"
      },
      {
        "nome": "guido",
        "dataNascita": "2001-01-22",
        "altezza": 196,
        "peso": 90.4,
        "comuneNascita": "Roma"
      },
      {
        "nome": "sara",
        "dataNascita": "2000-02-22",
        "altezza": 166,
        "peso": 70.4,
        "comuneNascita": "Roma"
      },
      {
        "nome": "giulia",
        "dataNascita": "1997-08-13",
        "altezza": 169,
        "peso": 68.3,
        "comuneNascita": "Milano"
      }
    ]
    ```

Se in Miller 6 si vuole un JSON Lines (formato molto consigliato), bisogna scegliere il [flag](flag.md#formati-file) `--ojsonl`:


!!! comando "mlr --icsv --ojsonl cat base_category.csv"

    ```json
    {"nome": "andy", "dataNascita": "1973-05-08", "altezza": 176, "peso": 86.5, "comuneNascita": "Roma"}
    {"nome": "chiara", "dataNascita": "1993-12-13", "altezza": 162, "peso": 58.3, "comuneNascita": "Milano"}
    {"nome": "guido", "dataNascita": "2001-01-22", "altezza": 196, "peso": 90.4, "comuneNascita": "Roma"}
    {"nome": "sara", "dataNascita": "2000-02-22", "altezza": 166, "peso": 70.4, "comuneNascita": "Roma"}
    {"nome": "giulia", "dataNascita": "1997-08-13", "altezza": 169, "peso": 68.3, "comuneNascita": "Milano"}
    ```

### PPRINT: Pretty-printed tabular

!!! comando "mlr --c2p --barred cat base_category.csv"

    ```
    +--------+-------------+---------+------+---------------+
    | nome   | dataNascita | altezza | peso | comuneNascita |
    +--------+-------------+---------+------+---------------+
    | andy   | 1973-05-08  | 176     | 86.5 | Roma          |
    | chiara | 1993-12-13  | 162     | 58.3 | Milano        |
    | guido  | 2001-01-22  | 196     | 90.4 | Roma          |
    | sara   | 2000-02-22  | 166     | 70.4 | Roma          |
    | giulia | 1997-08-13  | 169     | 68.3 | Milano        |
    +--------+-------------+---------+------+---------------+
    ```

### XTAB: Vertical tabular

!!! comando "mlr --c2x cat base_category.csv"

    ```
    nome          andy
    dataNascita   1973-05-08
    altezza       176
    peso          86.5
    comuneNascita Roma

    nome          chiara
    dataNascita   1993-12-13
    altezza       162
    peso          58.3
    comuneNascita Milano

    nome          guido
    dataNascita   2001-01-22
    altezza       196
    peso          90.4
    comuneNascita Roma

    nome          sara
    dataNascita   2000-02-22
    altezza       166
    peso          70.4
    comuneNascita Roma

    nome          giulia
    dataNascita   1997-08-13
    altezza       169
    peso          68.3
    comuneNascita Milano
    ```

### Markdown

!!! comando "mlr --c2m cat base_category.csv"

    ```
    | nome | dataNascita | altezza | peso | comuneNascita |
    | --- | --- | --- | --- | --- |
    | andy | 1973-05-08 | 176 | 86.5 | Roma |
    | chiara | 1993-12-13 | 162 | 58.3 | Milano |
    | guido | 2001-01-22 | 196 | 90.4 | Roma |
    | sara | 2000-02-22 | 166 | 70.4 | Roma |
    | giulia | 1997-08-13 | 169 | 68.3 | Milano |
    ```

## Casi speciali e consigli

### File CSV (anche TSV) senza riga di intestazione

In questo file non è presente la riga di intestazione.

``` title="input.csv"
1,861265,C,A,0.071
1,861265,C,A,0.148
1,861265,C,G,0.001
1,861265,C,G,0.108
1,861265,C,T,0
1,861265,C,T,0.216
2,193456,G,A,0.006
2,193456,G,A,0.094
2,193456,G,C,0.011
2,193456,G,C,0.152
2,193456,G,T,0.003
2,193456,G,T,0.056
```

Si può fare riferimento ai campi in modo da numerico, con un progressivo numerico di una unità a partire da 1, da sinistra verso destra. Il [`flag`](flag.md#csv) utile al caso è `-N`, che dà per implicito che non ci sia la riga di intestazione (e ne assegna una temporanea con campi numerici) e che non venga aggiunta in *output*.
<br>Quindi se da questo file vorrò estrarre le prime tre righe della prima colonna il comando sarà:

!!! comando "mlr --csv -N cut -f 1 then head -n 3 input.csv"

    ```
    1
    1
    1
    ```


