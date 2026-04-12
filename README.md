# Olist E-Commerce - Power BI Dashboard

## Descrizione
Report di Business Intelligence sviluppato come esercitazione di fine modulo del Master in Data Analytics. Il progetto analizza il dataset pubblico di [Olist Store](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), piattaforma e-commerce brasiliana, con dati di vendita dal 2016 al 2018.

## Obiettivi di Analisi
1. **Andamento degli ordini nel tempo per stato** - Conteggio ordini mese per mese con confronto anno precedente e variazione % YoY
2. **Andamento dei ricavi nel tempo per stato** - Ricavi totali mese per mese con confronto anno precedente e variazione % YoY
3. **Distribuzione del rating** - Analisi delle review per score, categoria prodotto e area geografica

## Struttura del Report

| Pagina | Contenuto |
|--------|-----------|
| **Andamento Ordini** | 3 Card KPI + Grafico Ordini vs PY + Variazione % mensile (verde/rosso) + Tabella per stato |
| **Andamento Ricavi** | 3 Card KPI + Grafico Ricavi vs PY + Variazione % mensile (verde/rosso) + Tabella per stato |
| **Distribuzione Rating** | Istogramma review score (1-5) + Rating medio per stato |
| **Analisi per Categoria** | Rating medio per categoria prodotto + Tabella dettaglio |
| **Dettaglio Stato** | Pagina drill-through con dettaglio per citta |

## Modello Dati (Star Schema)

```
DimCalendar ──1:N──> olist_orders_dataset <──N:1── olist_customers_dataset
                          |          |
                         1:N        1:N
                          |          |
              olist_order_items_d.  olist_order_reviews_d.
                          |
                         N:1
                          |
              olist_products_dataset
```

### Tabelle utilizzate
- **olist_order_items_dataset** (Fact Table) - order_id, order_item_id, product_id, price, freight_value, Ricavo
- **olist_orders_dataset** - order_id, customer_id, order_date, order_status
- **olist_customers_dataset** - customer_id, customer_city, customer_state
- **olist_products_dataset** - product_id, product_category_name, category_english
- **olist_order_reviews_dataset** - order_id, review_score
- **DimCalendar** - Date, Year, MonthName, MonthNumber, Quarter, YearMonth

### Misure DAX
| Misura | Formula |
|--------|---------|
| Ordini | `COUNT(olist_order_items_dataset[order_item_id])` |
| Ordini PY | `CALCULATE([Ordini], SAMEPERIODLASTYEAR(DimCalendar[Date]))` |
| Var% Ordini | `DIVIDE([Ordini] - [Ordini PY], [Ordini PY], BLANK())` |
| Ricavo Totale | `SUM(olist_order_items_dataset[Ricavo])` |
| Ricavo PY | `CALCULATE([Ricavo Totale], SAMEPERIODLASTYEAR(DimCalendar[Date]))` |
| Var% Ricavo | `DIVIDE([Ricavo Totale] - [Ricavo PY], [Ricavo PY], BLANK())` |
| Rating Medio | `AVERAGEX(olist_order_reviews_dataset, olist_order_reviews_dataset[review_score])` |

## Best Practice Applicate
- Riduzione del volume del dataset (rimozione colonne non necessarie)
- Modello dati a stella (Star Schema)
- Dimensione calendario dedicata (DimCalendar)
- Formattazione condizionale (verde/rosso) sulle variazioni percentuali
- Navigazione tra pagine tramite bottoni
- Drill-through per approfondimento per stato
- Layout professionale con sfondo, card stilizzate e titoli chiari

## Tecnologie
- **Power BI Desktop**
- **DAX** (Data Analysis Expressions)
- **Power Query** (M Language)

## File del Report
[Scarica il file Power BI (.pbix) da Google Drive](https://drive.google.com/file/d/15IKybe32Lmd3oMcLOjHd173aQpIHcZaP/view?usp=drive_link)

## Dataset
[Olist Brazilian E-Commerce Dataset - Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
