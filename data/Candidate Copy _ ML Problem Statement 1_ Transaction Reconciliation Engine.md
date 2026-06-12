# Problem Statement

# **Transaction Reconciliation Engine**

## 

## **Background Story**

Your company operates a payment platform. Every day, transactions are recorded by two independent systems.

* System A is the internal ledger.  
* System B is the bank or payment gateway report.

Due to network delays, rounding, naming differences, and time zone differences, the two systems do not match perfectly line by line. Your job is to build a reconciliation engine that automatically matches as many transactions as possible and reports what could not be matched.

---

## **Input**

You are given two lists of transactions.

Each transaction has:

* transaction\_id  
* merchant  
* amount  
* timestamp as string with timezone, for example 2025-01-12 10:31:05 \+0530

The two systems:

* may use different merchant naming conventions  
* may have small amount differences due to rounding or fees  
* may have small timestamp differences due to processing delays  
* may contain duplicates or near duplicates

You are also given:

* a merchant normalization mapping, for example:

  * {"AMZN MKTPLACE": "Amazon", "Amazon Marketplace": "Amazon"}

---

## **Matching Rules**

Two transactions can be matched if:

1. Merchant matches after normalization and cleanup

   * Case insensitive  
   * Strip spaces and punctuation  
   * Apply mapping table

2. Amount matches within tolerance:

   * absolute difference \<= 0.50 OR  
   * difference \<= 0.1 percent of the amount

3. Timestamp difference is within 2 minutes.

---

## **Ambiguity Rule**

There may be multiple possible matches for a transaction.

Your system must:

* maximize total number of matched pairs  
* if multiple matchings give same count, choose the one with minimum total time difference

Each transaction can be used in at most one match.

---

## **Output**

Return a reconciliation report containing:

1. List of matched pairs with:

   * transaction from A  
   * transaction from B  
   * time difference  
   * amount difference

2. List of unmatched transactions from A with reason:

   * no merchant match  
   * no amount match  
   * no timestamp match

3. List of unmatched transactions from B with reason.

4. Summary statistics:

   * total A count  
   * total B count  
   * matched count  
   * unmatched count

---

## **Constraints**

* Up to 200000 transactions per list.

---

## **What This Tests**

* Data normalization and cleaning  
* Sorting and indexing strategies  
* Two pointer or window scanning techniques  
* Greedy matching under constraints  
* Careful handling of edge cases  
* Designing debuggable output structures  
* Performance aware Python coding

---

## **What a Good Solution Looks Like**

* Preprocess and normalize merchants  
* Sort by merchant and time  
* Use time window scanning per merchant  
* Build candidate pools and choose best matches greedily  
* Produce structured report, not just raw lists

# input\_a.json

\[  
 {  
   "id": "a\_h246316",  
   "merchant": "Netflix.com",  
   "amount": 372.07,  
   "ts": "2026-01-12 12:13:44 \+0530"  
 },  
 {  
   "id": "a\_b131244",  
   "merchant": "NETFLIX,COM",  
   "amount": 48.03,  
   "ts": "2026-01-12 13:50:25 \+0530"  
 },  
 {  
   "id": "a\_r308496",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 255.15,  
   "ts": "2026-01-12 10:14:29 \+0530"  
 },  
 {  
   "id": "a\_s391704",  
   "merchant": "NETFLIX,COM",  
   "amount": 212.66,  
   "ts": "2026-01-12 14:05:19 \+0530"  
 },  
 {  
   "id": "a\_w543143",  
   "merchant": "Local Grocery",  
   "amount": 380.61,  
   "ts": "2026-01-12 11:27:11 \+0530"  
 },  
 {  
   "id": "a\_k207175",  
   "merchant": "STARBUCKS 123",  
   "amount": 111.58,  
   "ts": "2026-01-12 16:56:58 \+0530"  
 },  
 {  
   "id": "a\_t377370",  
   "merchant": "Amazon Marketplace",  
   "amount": 182.69,  
   "ts": "2026-01-12 13:07:50 \+0530"  
 },  
 {  
   "id": "a\_c678856",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 270.43,  
   "ts": "2026-01-12 13:26:43 \+0530"  
 },  
 {  
   "id": "a\_s301629",  
   "merchant": "STARBUCKS.123",  
   "amount": 443.3,  
   "ts": "2026-01-12 13:17:30 \+0530"  
 },  
 {  
   "id": "a\_j183667",  
   "merchant": "Netflix.com",  
   "amount": 332.33,  
   "ts": "2026-01-12 17:02:11 \+0530"  
 },  
 {  
   "id": "a\_u974628",  
   "merchant": "Local Grocery",  
   "amount": 193.16,  
   "ts": "2026-01-12 14:07:37 \+0530"  
 },  
 {  
   "id": "a\_i835911",  
   "merchant": "Starbucks Store, 123",  
   "amount": 180.86,  
   "ts": "2026-01-12 16:06:00 \+0530"  
 },  
 {  
   "id": "a\_r864544",  
   "merchant": "Coffee Shop 77",  
   "amount": 306.52,  
   "ts": "2026-01-12 11:33:27 \+0530"  
 },  
 {  
   "id": "a\_w684004",  
   "merchant": " starbucks 123 ",  
   "amount": 192.83,  
   "ts": "2026-01-12 15:49:31 \+0530"  
 },  
 {  
   "id": "a\_b340174",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 422.21,  
   "ts": "2026-01-12 17:03:47 \+0530"  
 },  
 {  
   "id": "a\_g694731",  
   "merchant": "Local-Grocery",  
   "amount": 203.58,  
   "ts": "2026-01-12 10:36:08 \+0530"  
 },  
 {  
   "id": "a\_m774079",  
   "merchant": "Local-Grocery",  
   "amount": 110.25,  
   "ts": "2026-01-12 14:32:38 \+0530"  
 },  
 {  
   "id": "a\_r665158",  
   "merchant": "Uber  Trip",  
   "amount": 74.12,  
   "ts": "2026-01-12 16:46:51 \+0530"  
 },  
 {  
   "id": "a\_m479580",  
   "merchant": "STARBUCKS.123",  
   "amount": 217.07,  
   "ts": "2026-01-12 15:18:41 \+0530"  
 },  
 {  
   "id": "a\_y149405",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 257.22,  
   "ts": "2026-01-12 10:49:38 \+0530"  
 },  
 {  
   "id": "a\_v542666",  
   "merchant": "LOCAL GROCERY",  
   "amount": 315.59,  
   "ts": "2026-01-12 17:12:34 \+0530"  
 },  
 {  
   "id": "a\_q363626",  
   "merchant": "uber trip",  
   "amount": 193.9,  
   "ts": "2026-01-12 14:15:37 \+0530"  
 },  
 {  
   "id": "a\_v663054",  
   "merchant": "Coffee Shop 77",  
   "amount": 341.74,  
   "ts": "2026-01-12 11:02:33 \+0530"  
 },  
 {  
   "id": "a\_f575763",  
   "merchant": "NETFLIX.COM",  
   "amount": 60.22,  
   "ts": "2026-01-12 13:57:26 \+0530"  
 },  
 {  
   "id": "a\_q898975",  
   "merchant": "amzn mktplace",  
   "amount": 438.55,  
   "ts": "2026-01-12 12:23:50 \+0530"  
 },  
 {  
   "id": "a\_u632323",  
   "merchant": "Amazon Marketplace",  
   "amount": 435.91,  
   "ts": "2026-01-12 12:42:59 \+0530"  
 },  
 {  
   "id": "a\_r916449",  
   "merchant": "UBER BV",  
   "amount": 190.09,  
   "ts": "2026-01-12 11:28:13 \+0530"  
 },  
 {  
   "id": "a\_a217301",  
   "merchant": "Coffee Shop 77",  
   "amount": 301.48,  
   "ts": "2026-01-12 14:26:50 \+0530"  
 },  
 {  
   "id": "a\_s182582",  
   "merchant": "Pharmacy 22",  
   "amount": 123.53,  
   "ts": "2026-01-12 12:11:32 \+0530"  
 },  
 {  
   "id": "a\_r903035",  
   "merchant": "Amazon   Marketplace",  
   "amount": 408.93,  
   "ts": "2026-01-12 16:55:23 \+0530"  
 },  
 {  
   "id": "a\_f377932",  
   "merchant": "amzn mktplace",  
   "amount": 240.26,  
   "ts": "2026-01-12 15:00:15 \+0530"  
 },  
 {  
   "id": "a\_r891952",  
   "merchant": "UBER, BV",  
   "amount": 214.45,  
   "ts": "2026-01-12 11:55:40 \+0530"  
 },  
 {  
   "id": "a\_v781446",  
   "merchant": "NETFLIX",  
   "amount": 357.91,  
   "ts": "2026-01-12 13:37:54 \+0530"  
 },  
 {  
   "id": "a\_h167136",  
   "merchant": "STARBUCKS.123",  
   "amount": 228.49,  
   "ts": "2026-01-12 12:15:23 \+0530"  
 },  
 {  
   "id": "a\_h107540",  
   "merchant": "STARBUCKS.123",  
   "amount": 279.19,  
   "ts": "2026-01-12 15:21:22 \+0530"  
 },  
 {  
   "id": "a\_b446479",  
   "merchant": "amzn mktplace",  
   "amount": 34.14,  
   "ts": "2026-01-12 10:36:48 \+0530"  
 },  
 {  
   "id": "a\_g665427",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 142.85,  
   "ts": "2026-01-12 14:25:06 \+0530"  
 },  
 {  
   "id": "a\_z595948",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 290.22,  
   "ts": "2026-01-12 12:12:42 \+0530"  
 },  
 {  
   "id": "a\_n471507",  
   "merchant": "LOCAL GROCERY",  
   "amount": 51.69,  
   "ts": "2026-01-12 15:59:53 \+0530"  
 },  
 {  
   "id": "a\_v785197",  
   "merchant": " starbucks 123 ",  
   "amount": 432.6,  
   "ts": "2026-01-12 10:29:35 \+0530"  
 },  
 {  
   "id": "a\_k939482",  
   "merchant": "Coffee Shop 77",  
   "amount": 35.0,  
   "ts": "2026-01-12 16:37:42 \+0530"  
 },  
 {  
   "id": "a\_o246991",  
   "merchant": "LOCAL GROCERY",  
   "amount": 99.84,  
   "ts": "2026-01-12 14:52:53 \+0530"  
 },  
 {  
   "id": "a\_c564656",  
   "merchant": "Starbucks Store, 123",  
   "amount": 234.0,  
   "ts": "2026-01-12 17:57:35 \+0530"  
 },  
 {  
   "id": "a\_r976638",  
   "merchant": "local grocery",  
   "amount": 53.46,  
   "ts": "2026-01-12 15:56:09 \+0530"  
 },  
 {  
   "id": "a\_h274389",  
   "merchant": "Amazon Marketplace",  
   "amount": 463.55,  
   "ts": "2026-01-12 17:43:30 \+0530"  
 },  
 {  
   "id": "a\_b272634",  
   "merchant": " starbucks 123 ",  
   "amount": 110.8,  
   "ts": "2026-01-12 13:39:01 \+0530"  
 },  
 {  
   "id": "a\_z577110",  
   "merchant": " starbucks 123 ",  
   "amount": 136.28,  
   "ts": "2026-01-12 17:08:12 \+0530"  
 },  
 {  
   "id": "a\_e299122",  
   "merchant": "STARBUCKS.123",  
   "amount": 332.63,  
   "ts": "2026-01-12 14:25:47 \+0530"  
 },  
 {  
   "id": "a\_b884309",  
   "merchant": "Starbucks Store 123",  
   "amount": 291.69,  
   "ts": "2026-01-12 14:56:06 \+0530"  
 },  
 {  
   "id": "a\_q265080",  
   "merchant": "Starbucks Store 123",  
   "amount": 294.17,  
   "ts": "2026-01-12 14:34:37 \+0530"  
 },  
 {  
   "id": "a\_c723939",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 44.66,  
   "ts": "2026-01-12 11:41:29 \+0530"  
 },  
 {  
   "id": "a\_h707040",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 204.87,  
   "ts": "2026-01-12 15:11:07 \+0530"  
 },  
 {  
   "id": "a\_s692683",  
   "merchant": "UBER, BV",  
   "amount": 45.58,  
   "ts": "2026-01-12 15:59:00 \+0530"  
 },  
 {  
   "id": "a\_k350280",  
   "merchant": "Uber  Trip",  
   "amount": 106.11,  
   "ts": "2026-01-12 16:31:08 \+0530"  
 },  
 {  
   "id": "a\_o431535",  
   "merchant": "STARBUCKS 123",  
   "amount": 337.49,  
   "ts": "2026-01-12 12:43:50 \+0530"  
 },  
 {  
   "id": "a\_s204837",  
   "merchant": "Coffee Shop 77",  
   "amount": 9.61,  
   "ts": "2026-01-12 15:39:14 \+0530"  
 },  
 {  
   "id": "a\_l172132",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 255.41,  
   "ts": "2026-01-12 11:12:20 \+0530"  
 },  
 {  
   "id": "a\_r837715",  
   "merchant": "Local-Grocery",  
   "amount": 146.07,  
   "ts": "2026-01-12 13:59:18 \+0530"  
 },  
 {  
   "id": "a\_r413921",  
   "merchant": "STARBUCKS.123",  
   "amount": 8.87,  
   "ts": "2026-01-12 17:26:14 \+0530"  
 },  
 {  
   "id": "a\_i221035",  
   "merchant": "Coffee Shop 77",  
   "amount": 469.77,  
   "ts": "2026-01-12 11:13:20 \+0530"  
 },  
 {  
   "id": "a\_t320861",  
   "merchant": "local grocery",  
   "amount": 81.95,  
   "ts": "2026-01-12 12:33:52 \+0530"  
 },  
 {  
   "id": "a\_i629959",  
   "merchant": "NETFLIX",  
   "amount": 345.31,  
   "ts": "2026-01-12 17:45:48 \+0530"  
 },  
 {  
   "id": "a\_i146228",  
   "merchant": "Uber Trip",  
   "amount": 50.69,  
   "ts": "2026-01-12 13:51:19 \+0530"  
 },  
 {  
   "id": "a\_f877236",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 320.37,  
   "ts": "2026-01-12 12:23:03 \+0530"  
 },  
 {  
   "id": "a\_c824586",  
   "merchant": "uber trip",  
   "amount": 282.65,  
   "ts": "2026-01-12 11:01:05 \+0530"  
 },  
 {  
   "id": "a\_s679364",  
   "merchant": "local grocery",  
   "amount": 22.83,  
   "ts": "2026-01-12 13:21:38 \+0530"  
 },  
 {  
   "id": "a\_z141832",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 25.71,  
   "ts": "2026-01-12 13:19:08 \+0530"  
 },  
 {  
   "id": "a\_d470858",  
   "merchant": "LOCAL GROCERY",  
   "amount": 342.62,  
   "ts": "2026-01-12 16:04:14 \+0530"  
 },  
 {  
   "id": "a\_e348237",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 487.05,  
   "ts": "2026-01-12 16:49:18 \+0530"  
 },  
 {  
   "id": "a\_f872343",  
   "merchant": "LOCAL GROCERY",  
   "amount": 441.3,  
   "ts": "2026-01-12 10:13:32 \+0530"  
 },  
 {  
   "id": "a\_x950132",  
   "merchant": "Fuel Station 9",  
   "amount": 402.11,  
   "ts": "2026-01-12 17:51:53 \+0530"  
 },  
 {  
   "id": "a\_m140605",  
   "merchant": "STARBUCKS 123",  
   "amount": 394.75,  
   "ts": "2026-01-12 10:59:02 \+0530"  
 },  
 {  
   "id": "a\_l420015",  
   "merchant": "LOCAL GROCERY",  
   "amount": 103.79,  
   "ts": "2026-01-12 14:11:23 \+0530"  
 },  
 {  
   "id": "a\_g517821",  
   "merchant": "LOCAL GROCERY",  
   "amount": 115.35,  
   "ts": "2026-01-12 16:00:27 \+0530"  
 },  
 {  
   "id": "a\_l772642",  
   "merchant": "Starbucks Store 123",  
   "amount": 483.61,  
   "ts": "2026-01-12 12:32:26 \+0530"  
 },  
 {  
   "id": "a\_d373903",  
   "merchant": "UBER, BV",  
   "amount": 168.92,  
   "ts": "2026-01-12 10:15:04 \+0530"  
 },  
 {  
   "id": "a\_n462479",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 23.94,  
   "ts": "2026-01-12 15:25:48 \+0530"  
 },  
 {  
   "id": "a\_d503906",  
   "merchant": "NETFLIX.COM",  
   "amount": 221.01,  
   "ts": "2026-01-12 14:39:18 \+0530"  
 },  
 {  
   "id": "a\_n101773",  
   "merchant": "LOCAL GROCERY",  
   "amount": 131.09,  
   "ts": "2026-01-12 16:27:05 \+0530"  
 },  
 {  
   "id": "a\_x803204",  
   "merchant": "UBER, BV",  
   "amount": 344.99,  
   "ts": "2026-01-12 16:45:12 \+0530"  
 },  
 {  
   "id": "a\_k753425",  
   "merchant": "Amazon   Marketplace",  
   "amount": 39.64,  
   "ts": "2026-01-12 16:02:45 \+0530"  
 },  
 {  
   "id": "a\_q424308",  
   "merchant": "Starbucks Store 123",  
   "amount": 361.28,  
   "ts": "2026-01-12 12:44:00 \+0530"  
 },  
 {  
   "id": "a\_r233470",  
   "merchant": "NETFLIX.COM",  
   "amount": 204.19,  
   "ts": "2026-01-12 12:41:28 \+0530"  
 },  
 {  
   "id": "a\_x282480",  
   "merchant": "amzn mktplace",  
   "amount": 470.48,  
   "ts": "2026-01-12 16:09:54 \+0530"  
 },  
 {  
   "id": "a\_a418635",  
   "merchant": "Uber  Trip",  
   "amount": 206.01,  
   "ts": "2026-01-12 17:35:19 \+0530"  
 },  
 {  
   "id": "a\_u437902",  
   "merchant": " starbucks 123 ",  
   "amount": 393.95,  
   "ts": "2026-01-12 15:31:19 \+0530"  
 },  
 {  
   "id": "a\_p932291",  
   "merchant": "uber trip",  
   "amount": 339.44,  
   "ts": "2026-01-12 14:39:10 \+0530"  
 },  
 {  
   "id": "a\_q796101",  
   "merchant": "LOCAL GROCERY",  
   "amount": 331.13,  
   "ts": "2026-01-12 12:34:59 \+0530"  
 },  
 {  
   "id": "a\_h805477",  
   "merchant": "NETFLIX.COM",  
   "amount": 51.23,  
   "ts": "2026-01-12 16:50:13 \+0530"  
 },  
 {  
   "id": "a\_h598216",  
   "merchant": "STARBUCKS 123",  
   "amount": 77.94,  
   "ts": "2026-01-12 10:25:14 \+0530"  
 },  
 {  
   "id": "a\_s303880",  
   "merchant": "Uber Trip",  
   "amount": 230.43,  
   "ts": "2026-01-12 15:43:56 \+0530"  
 },  
 {  
   "id": "a\_e787926",  
   "merchant": " netflix com ",  
   "amount": 249.72,  
   "ts": "2026-01-12 12:13:14 \+0530"  
 },  
 {  
   "id": "a\_f943170",  
   "merchant": "Netflix.com",  
   "amount": 390.32,  
   "ts": "2026-01-12 11:59:30 \+0530"  
 },  
 {  
   "id": "a\_h989545",  
   "merchant": "Restaurant ABC",  
   "amount": 234.95,  
   "ts": "2026-01-12 15:04:25 \+0530"  
 },  
 {  
   "id": "a\_q686075",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 401.7,  
   "ts": "2026-01-12 16:04:35 \+0530"  
 },  
 {  
   "id": "a\_q547470",  
   "merchant": "uber trip",  
   "amount": 308.26,  
   "ts": "2026-01-12 16:32:49 \+0530"  
 },  
 {  
   "id": "a\_x597732",  
   "merchant": "local grocery",  
   "amount": 225.73,  
   "ts": "2026-01-12 11:26:55 \+0530"  
 },  
 {  
   "id": "a\_y915451",  
   "merchant": "UBER BV",  
   "amount": 420.73,  
   "ts": "2026-01-12 12:31:26 \+0530"  
 },  
 {  
   "id": "a\_w399607",  
   "merchant": "UBER BV",  
   "amount": 140.93,  
   "ts": "2026-01-12 10:42:18 \+0530"  
 },  
 {  
   "id": "a\_c245095",  
   "merchant": "Starbucks Store, 123",  
   "amount": 163.26,  
   "ts": "2026-01-12 14:54:59 \+0530"  
 },  
 {  
   "id": "a\_g167348",  
   "merchant": "Amazon   Marketplace",  
   "amount": 348.51,  
   "ts": "2026-01-12 16:25:47 \+0530"  
 },  
 {  
   "id": "a\_b316881",  
   "merchant": "Starbucks Store, 123",  
   "amount": 273.59,  
   "ts": "2026-01-12 13:47:04 \+0530"  
 },  
 {  
   "id": "a\_w120480",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 452.9,  
   "ts": "2026-01-12 15:18:59 \+0530"  
 },  
 {  
   "id": "a\_l413118",  
   "merchant": "local grocery",  
   "amount": 193.29,  
   "ts": "2026-01-12 10:03:13 \+0530"  
 },  
 {  
   "id": "a\_r938722",  
   "merchant": " netflix com ",  
   "amount": 271.42,  
   "ts": "2026-01-12 16:41:12 \+0530"  
 },  
 {  
   "id": "a\_n609232",  
   "merchant": "UBER BV",  
   "amount": 246.67,  
   "ts": "2026-01-12 12:29:03 \+0530"  
 },  
 {  
   "id": "a\_m859359",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 336.09,  
   "ts": "2026-01-12 17:15:53 \+0530"  
 },  
 {  
   "id": "a\_r128276",  
   "merchant": "Amazon   Marketplace",  
   "amount": 460.17,  
   "ts": "2026-01-12 15:39:50 \+0530"  
 },  
 {  
   "id": "a\_c773971",  
   "merchant": "local grocery",  
   "amount": 284.37,  
   "ts": "2026-01-12 10:14:48 \+0530"  
 },  
 {  
   "id": "a\_m443254",  
   "merchant": " starbucks 123 ",  
   "amount": 94.96,  
   "ts": "2026-01-12 12:22:04 \+0530"  
 },  
 {  
   "id": "a\_i888539",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 172.06,  
   "ts": "2026-01-12 13:27:03 \+0530"  
 },  
 {  
   "id": "a\_p120324",  
   "merchant": "Fuel Station 9",  
   "amount": 129.87,  
   "ts": "2026-01-12 10:44:43 \+0530"  
 },  
 {  
   "id": "a\_h781725",  
   "merchant": "Netflix.com",  
   "amount": 499.95,  
   "ts": "2026-01-12 13:11:07 \+0530"  
 },  
 {  
   "id": "a\_h309044",  
   "merchant": "amzn mktplace",  
   "amount": 24.93,  
   "ts": "2026-01-12 10:16:56 \+0530"  
 },  
 {  
   "id": "a\_p801978",  
   "merchant": "local grocery",  
   "amount": 80.43,  
   "ts": "2026-01-12 11:08:56 \+0530"  
 },  
 {  
   "id": "a\_y486814",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 235.19,  
   "ts": "2026-01-12 12:19:56 \+0530"  
 },  
 {  
   "id": "a\_d915573",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 482.05,  
   "ts": "2026-01-12 16:32:19 \+0530"  
 },  
 {  
   "id": "a\_j703766",  
   "merchant": "Local-Grocery",  
   "amount": 58.51,  
   "ts": "2026-01-12 10:14:01 \+0530"  
 },  
 {  
   "id": "a\_g179688",  
   "merchant": " netflix com ",  
   "amount": 201.34,  
   "ts": "2026-01-12 16:30:29 \+0530"  
 },  
 {  
   "id": "a\_j991597",  
   "merchant": "UBER BV",  
   "amount": 55.44,  
   "ts": "2026-01-12 17:01:52 \+0530"  
 },  
 {  
   "id": "a\_z143064",  
   "merchant": "Netflix.com",  
   "amount": 399.13,  
   "ts": "2026-01-12 15:09:04 \+0530"  
 },  
 {  
   "id": "a\_q778998",  
   "merchant": " starbucks 123 ",  
   "amount": 332.45,  
   "ts": "2026-01-12 10:37:39 \+0530"  
 },  
 {  
   "id": "a\_n479781",  
   "merchant": " starbucks 123 ",  
   "amount": 412.05,  
   "ts": "2026-01-12 10:57:38 \+0530"  
 },  
 {  
   "id": "a\_f869440",  
   "merchant": " netflix com ",  
   "amount": 355.12,  
   "ts": "2026-01-12 13:57:50 \+0530"  
 },  
 {  
   "id": "a\_y606983",  
   "merchant": "Uber  Trip",  
   "amount": 309.88,  
   "ts": "2026-01-12 14:53:54 \+0530"  
 },  
 {  
   "id": "a\_h971084",  
   "merchant": "UBER, BV",  
   "amount": 137.86,  
   "ts": "2026-01-12 17:45:08 \+0530"  
 },  
 {  
   "id": "a\_y587282",  
   "merchant": "Pharmacy 22",  
   "amount": 441.41,  
   "ts": "2026-01-12 12:13:10 \+0530"  
 },  
 {  
   "id": "a\_k290672",  
   "merchant": "uber trip",  
   "amount": 171.52,  
   "ts": "2026-01-12 14:29:57 \+0530"  
 },  
 {  
   "id": "a\_i725113",  
   "merchant": "Uber  Trip",  
   "amount": 399.93,  
   "ts": "2026-01-12 13:05:52 \+0530"  
 },  
 {  
   "id": "a\_g189771",  
   "merchant": "NETFLIX.COM",  
   "amount": 280.12,  
   "ts": "2026-01-12 14:42:08 \+0530"  
 },  
 {  
   "id": "a\_h824156",  
   "merchant": " starbucks 123 ",  
   "amount": 246.85,  
   "ts": "2026-01-12 16:54:03 \+0530"  
 },  
 {  
   "id": "a\_c408528",  
   "merchant": "uber trip",  
   "amount": 226.85,  
   "ts": "2026-01-12 10:09:25 \+0530"  
 },  
 {  
   "id": "a\_l596249",  
   "merchant": "STARBUCKS 123",  
   "amount": 156.57,  
   "ts": "2026-01-12 15:17:36 \+0530"  
 },  
 {  
   "id": "a\_r446859",  
   "merchant": "Uber  Trip",  
   "amount": 215.62,  
   "ts": "2026-01-12 16:47:22 \+0530"  
 },  
 {  
   "id": "a\_h226516",  
   "merchant": " starbucks 123 ",  
   "amount": 139.1,  
   "ts": "2026-01-12 12:17:17 \+0530"  
 },  
 {  
   "id": "a\_y823688",  
   "merchant": "NETFLIX.COM",  
   "amount": 64.18,  
   "ts": "2026-01-12 14:52:39 \+0530"  
 },  
 {  
   "id": "a\_x718226",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 370.61,  
   "ts": "2026-01-12 12:31:00 \+0530"  
 },  
 {  
   "id": "a\_g410636",  
   "merchant": "Restaurant ABC",  
   "amount": 300.41,  
   "ts": "2026-01-12 10:54:54 \+0530"  
 },  
 {  
   "id": "a\_r232731",  
   "merchant": "STARBUCKS 123",  
   "amount": 154.62,  
   "ts": "2026-01-12 16:26:40 \+0530"  
 },  
 {  
   "id": "a\_e768854",  
   "merchant": "Starbucks Store 123",  
   "amount": 278.91,  
   "ts": "2026-01-12 16:20:52 \+0530"  
 },  
 {  
   "id": "a\_s398151",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 55.78,  
   "ts": "2026-01-12 10:06:41 \+0530"  
 },  
 {  
   "id": "a\_i600935",  
   "merchant": "uber trip",  
   "amount": 173.65,  
   "ts": "2026-01-12 10:28:03 \+0530"  
 },  
 {  
   "id": "a\_s760021",  
   "merchant": "Amazon Marketplace",  
   "amount": 203.35,  
   "ts": "2026-01-12 10:40:27 \+0530"  
 },  
 {  
   "id": "a\_j189318",  
   "merchant": "NETFLIX",  
   "amount": 78.85,  
   "ts": "2026-01-12 15:07:23 \+0530"  
 },  
 {  
   "id": "a\_t725084",  
   "merchant": "Coffee Shop 77",  
   "amount": 281.25,  
   "ts": "2026-01-12 13:47:17 \+0530"  
 },  
 {  
   "id": "a\_o564226",  
   "merchant": "LOCAL GROCERY",  
   "amount": 388.97,  
   "ts": "2026-01-12 13:27:44 \+0530"  
 },  
 {  
   "id": "a\_s751194",  
   "merchant": "STARBUCKS.123",  
   "amount": 495.07,  
   "ts": "2026-01-12 12:46:46 \+0530"  
 },  
 {  
   "id": "a\_g755900",  
   "merchant": "amzn mktplace",  
   "amount": 54.12,  
   "ts": "2026-01-12 16:56:33 \+0530"  
 },  
 {  
   "id": "a\_f678805",  
   "merchant": "amzn mktplace",  
   "amount": 45.19,  
   "ts": "2026-01-12 12:10:59 \+0530"  
 },  
 {  
   "id": "a\_t592738",  
   "merchant": "Amazon Marketplace",  
   "amount": 207.22,  
   "ts": "2026-01-12 16:16:29 \+0530"  
 },  
 {  
   "id": "a\_w576086",  
   "merchant": "STARBUCKS 123",  
   "amount": 147.61,  
   "ts": "2026-01-12 12:34:24 \+0530"  
 },  
 {  
   "id": "a\_z755420",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 462.34,  
   "ts": "2026-01-12 17:10:15 \+0530"  
 },  
 {  
   "id": "a\_h779094",  
   "merchant": "UBER BV",  
   "amount": 215.44,  
   "ts": "2026-01-12 14:57:24 \+0530"  
 },  
 {  
   "id": "a\_b273982",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 414.18,  
   "ts": "2026-01-12 10:38:59 \+0530"  
 },  
 {  
   "id": "a\_j560485",  
   "merchant": "local grocery",  
   "amount": 375.73,  
   "ts": "2026-01-12 15:10:51 \+0530"  
 },  
 {  
   "id": "a\_i624783",  
   "merchant": "amzn mktplace",  
   "amount": 155.53,  
   "ts": "2026-01-12 13:39:49 \+0530"  
 },  
 {  
   "id": "a\_n870214",  
   "merchant": "uber trip",  
   "amount": 44.82,  
   "ts": "2026-01-12 10:21:46 \+0530"  
 },  
 {  
   "id": "a\_v976566",  
   "merchant": "Starbucks Store, 123",  
   "amount": 17.8,  
   "ts": "2026-01-12 12:05:01 \+0530"  
 },  
 {  
   "id": "a\_v961322",  
   "merchant": "local grocery",  
   "amount": 475.64,  
   "ts": "2026-01-12 16:57:39 \+0530"  
 },  
 {  
   "id": "a\_p644173",  
   "merchant": "Starbucks Store 123",  
   "amount": 382.74,  
   "ts": "2026-01-12 11:35:40 \+0530"  
 },  
 {  
   "id": "a\_n765620",  
   "merchant": "NETFLIX.COM",  
   "amount": 94.84,  
   "ts": "2026-01-12 15:19:40 \+0530"  
 },  
 {  
   "id": "a\_k436653",  
   "merchant": "Local Grocery",  
   "amount": 237.66,  
   "ts": "2026-01-12 13:43:00 \+0530"  
 },  
 {  
   "id": "a\_p402218",  
   "merchant": "NETFLIX",  
   "amount": 168.25,  
   "ts": "2026-01-12 16:18:50 \+0530"  
 },  
 {  
   "id": "a\_b576919",  
   "merchant": " netflix com ",  
   "amount": 407.72,  
   "ts": "2026-01-12 15:00:25 \+0530"  
 },  
 {  
   "id": "a\_m639593",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 165.02,  
   "ts": "2026-01-12 17:01:54 \+0530"  
 },  
 {  
   "id": "a\_o533321",  
   "merchant": "Local Grocery",  
   "amount": 330.54,  
   "ts": "2026-01-12 14:56:19 \+0530"  
 },  
 {  
   "id": "a\_p755788",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 184.08,  
   "ts": "2026-01-12 16:53:07 \+0530"  
 },  
 {  
   "id": "a\_e402020",  
   "merchant": "Uber Trip",  
   "amount": 105.76,  
   "ts": "2026-01-12 14:59:56 \+0530"  
 },  
 {  
   "id": "a\_t938141",  
   "merchant": " starbucks 123 ",  
   "amount": 65.11,  
   "ts": "2026-01-12 15:44:04 \+0530"  
 },  
 {  
   "id": "a\_r527809",  
   "merchant": "STARBUCKS 123",  
   "amount": 158.82,  
   "ts": "2026-01-12 10:07:28 \+0530"  
 },  
 {  
   "id": "a\_u972565",  
   "merchant": "Amazon Marketplace",  
   "amount": 233.42,  
   "ts": "2026-01-12 11:04:09 \+0530"  
 },  
 {  
   "id": "a\_i535673",  
   "merchant": "amzn mktplace",  
   "amount": 149.47,  
   "ts": "2026-01-12 16:25:14 \+0530"  
 },  
 {  
   "id": "a\_e502219",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 125.65,  
   "ts": "2026-01-12 15:01:03 \+0530"  
 },  
 {  
   "id": "a\_c389666",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 256.58,  
   "ts": "2026-01-12 11:14:33 \+0530"  
 },  
 {  
   "id": "a\_q380150",  
   "merchant": " netflix com ",  
   "amount": 173.24,  
   "ts": "2026-01-12 17:10:11 \+0530"  
 },  
 {  
   "id": "a\_s708093",  
   "merchant": "Local-Grocery",  
   "amount": 364.4,  
   "ts": "2026-01-12 17:37:22 \+0530"  
 },  
 {  
   "id": "a\_r607868",  
   "merchant": "Fuel Station 9",  
   "amount": 433.3,  
   "ts": "2026-01-12 14:03:52 \+0530"  
 },  
 {  
   "id": "a\_o437408",  
   "merchant": "STARBUCKS.123",  
   "amount": 382.62,  
   "ts": "2026-01-12 13:26:00 \+0530"  
 },  
 {  
   "id": "a\_y530728",  
   "merchant": "LOCAL GROCERY",  
   "amount": 288.04,  
   "ts": "2026-01-12 12:07:33 \+0530"  
 },  
 {  
   "id": "a\_m504832",  
   "merchant": "amzn mktplace",  
   "amount": 239.12,  
   "ts": "2026-01-12 17:23:08 \+0530"  
 },  
 {  
   "id": "a\_e626690",  
   "merchant": "Ecomm Seller X",  
   "amount": 250.19,  
   "ts": "2026-01-12 10:20:13 \+0530"  
 },  
 {  
   "id": "a\_o204555",  
   "merchant": "Pharmacy 22",  
   "amount": 435.5,  
   "ts": "2026-01-12 17:57:25 \+0530"  
 },  
 {  
   "id": "a\_n786587",  
   "merchant": "uber trip",  
   "amount": 12.6,  
   "ts": "2026-01-12 11:18:43 \+0530"  
 },  
 {  
   "id": "a\_k753534",  
   "merchant": "Coffee Shop 77",  
   "amount": 237.4,  
   "ts": "2026-01-12 12:24:44 \+0530"  
 },  
 {  
   "id": "a\_v659543",  
   "merchant": "Netflix.com",  
   "amount": 426.62,  
   "ts": "2026-01-12 17:45:29 \+0530"  
 },  
 {  
   "id": "a\_p667296",  
   "merchant": "Starbucks Store, 123",  
   "amount": 315.27,  
   "ts": "2026-01-12 16:54:44 \+0530"  
 },  
 {  
   "id": "a\_j338536",  
   "merchant": "Amazon Marketplace",  
   "amount": 121.21,  
   "ts": "2026-01-12 16:13:46 \+0530"  
 },  
 {  
   "id": "a\_u838083",  
   "merchant": " netflix com ",  
   "amount": 489.28,  
   "ts": "2026-01-12 16:55:15 \+0530"  
 },  
 {  
   "id": "a\_a148225",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 87.35,  
   "ts": "2026-01-12 12:43:32 \+0530"  
 },  
 {  
   "id": "a\_n252640",  
   "merchant": "Starbucks Store 123",  
   "amount": 150.22,  
   "ts": "2026-01-12 13:24:42 \+0530"  
 },  
 {  
   "id": "a\_f278240",  
   "merchant": " starbucks 123 ",  
   "amount": 285.13,  
   "ts": "2026-01-12 17:12:49 \+0530"  
 },  
 {  
   "id": "a\_v352528",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 436.07,  
   "ts": "2026-01-12 15:38:26 \+0530"  
 },  
 {  
   "id": "a\_u366314",  
   "merchant": "UBER, BV",  
   "amount": 75.84,  
   "ts": "2026-01-12 14:11:50 \+0530"  
 },  
 {  
   "id": "a\_j810526",  
   "merchant": "Uber Trip",  
   "amount": 449.96,  
   "ts": "2026-01-12 14:14:05 \+0530"  
 },  
 {  
   "id": "a\_s413685",  
   "merchant": "Uber Trip",  
   "amount": 223.66,  
   "ts": "2026-01-12 13:08:43 \+0530"  
 },  
 {  
   "id": "a\_j308894",  
   "merchant": " netflix com ",  
   "amount": 346.71,  
   "ts": "2026-01-12 14:09:29 \+0530"  
 },  
 {  
   "id": "a\_s476395",  
   "merchant": "Fuel Station 9",  
   "amount": 57.79,  
   "ts": "2026-01-12 13:28:16 \+0530"  
 },  
 {  
   "id": "a\_v515032",  
   "merchant": "Uber  Trip",  
   "amount": 15.86,  
   "ts": "2026-01-12 17:33:18 \+0530"  
 },  
 {  
   "id": "a\_x151426",  
   "merchant": "STARBUCKS.123",  
   "amount": 433.21,  
   "ts": "2026-01-12 17:05:03 \+0530"  
 },  
 {  
   "id": "a\_y937665",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 417.26,  
   "ts": "2026-01-12 12:36:18 \+0530"  
 },  
 {  
   "id": "a\_t362801",  
   "merchant": "Starbucks Store, 123",  
   "amount": 113.41,  
   "ts": "2026-01-12 11:43:50 \+0530"  
 },  
 {  
   "id": "a\_u141337",  
   "merchant": "NETFLIX",  
   "amount": 315.98,  
   "ts": "2026-01-12 15:42:40 \+0530"  
 },  
 {  
   "id": "a\_x237795",  
   "merchant": " starbucks 123 ",  
   "amount": 21.52,  
   "ts": "2026-01-12 13:19:15 \+0530"  
 },  
 {  
   "id": "a\_f310548",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 166.73,  
   "ts": "2026-01-12 13:46:55 \+0530"  
 },  
 {  
   "id": "a\_q626169",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 438.85,  
   "ts": "2026-01-12 13:19:48 \+0530"  
 },  
 {  
   "id": "a\_p945664",  
   "merchant": "LOCAL GROCERY",  
   "amount": 132.2,  
   "ts": "2026-01-12 17:30:23 \+0530"  
 },  
 {  
   "id": "a\_c247594",  
   "merchant": "Starbucks Store, 123",  
   "amount": 403.1,  
   "ts": "2026-01-12 14:15:46 \+0530"  
 },  
 {  
   "id": "a\_v516716",  
   "merchant": "NETFLIX",  
   "amount": 430.77,  
   "ts": "2026-01-12 16:35:31 \+0530"  
 },  
 {  
   "id": "a\_m114594",  
   "merchant": "Restaurant ABC",  
   "amount": 186.06,  
   "ts": "2026-01-12 17:11:49 \+0530"  
 },  
 {  
   "id": "a\_x805000",  
   "merchant": "Starbucks Store 123",  
   "amount": 230.12,  
   "ts": "2026-01-12 16:07:27 \+0530"  
 },  
 {  
   "id": "a\_d807617",  
   "merchant": " starbucks 123 ",  
   "amount": 412.1,  
   "ts": "2026-01-12 13:22:52 \+0530"  
 },  
 {  
   "id": "a\_k739717",  
   "merchant": "Starbucks Store 123",  
   "amount": 311.68,  
   "ts": "2026-01-12 15:06:36 \+0530"  
 },  
 {  
   "id": "a\_w416869",  
   "merchant": "Starbucks Store 123",  
   "amount": 319.52,  
   "ts": "2026-01-12 14:13:33 \+0530"  
 },  
 {  
   "id": "a\_j616554",  
   "merchant": "Netflix.com",  
   "amount": 74.19,  
   "ts": "2026-01-12 10:20:19 \+0530"  
 },  
 {  
   "id": "a\_m575679",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 444.19,  
   "ts": "2026-01-12 11:14:05 \+0530"  
 },  
 {  
   "id": "a\_x262029",  
   "merchant": "STARBUCKS.123",  
   "amount": 212.47,  
   "ts": "2026-01-12 16:45:22 \+0530"  
 },  
 {  
   "id": "a\_n393328",  
   "merchant": "Local Grocery",  
   "amount": 417.68,  
   "ts": "2026-01-12 15:36:13 \+0530"  
 },  
 {  
   "id": "a\_h997004",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 112.55,  
   "ts": "2026-01-12 14:02:51 \+0530"  
 },  
 {  
   "id": "a\_l163491",  
   "merchant": "Starbucks Store, 123",  
   "amount": 274.51,  
   "ts": "2026-01-12 15:52:09 \+0530"  
 },  
 {  
   "id": "a\_o196126",  
   "merchant": "STARBUCKS 123",  
   "amount": 484.26,  
   "ts": "2026-01-12 17:43:08 \+0530"  
 },  
 {  
   "id": "a\_z449709",  
   "merchant": "NETFLIX,COM",  
   "amount": 485.69,  
   "ts": "2026-01-12 10:27:37 \+0530"  
 },  
 {  
   "id": "a\_c970127",  
   "merchant": "STARBUCKS 123",  
   "amount": 394.55,  
   "ts": "2026-01-12 11:52:04 \+0530"  
 },  
 {  
   "id": "a\_h444519",  
   "merchant": "NETFLIX",  
   "amount": 295.26,  
   "ts": "2026-01-12 17:24:02 \+0530"  
 },  
 {  
   "id": "a\_e236306",  
   "merchant": "NETFLIX,COM",  
   "amount": 6.4,  
   "ts": "2026-01-12 17:48:48 \+0530"  
 },  
 {  
   "id": "a\_a238205",  
   "merchant": "UBER BV",  
   "amount": 59.41,  
   "ts": "2026-01-12 17:53:11 \+0530"  
 },  
 {  
   "id": "a\_f378258",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 296.48,  
   "ts": "2026-01-12 10:08:37 \+0530"  
 },  
 {  
   "id": "a\_x166645",  
   "merchant": "amzn mktplace",  
   "amount": 213.38,  
   "ts": "2026-01-12 11:02:04 \+0530"  
 },  
 {  
   "id": "a\_o628313",  
   "merchant": "Uber  Trip",  
   "amount": 259.05,  
   "ts": "2026-01-12 10:59:33 \+0530"  
 },  
 {  
   "id": "a\_v646781",  
   "merchant": "STARBUCKS.123",  
   "amount": 26.47,  
   "ts": "2026-01-12 17:07:44 \+0530"  
 },  
 {  
   "id": "a\_m546980",  
   "merchant": "Starbucks Store 123",  
   "amount": 35.11,  
   "ts": "2026-01-12 14:21:35 \+0530"  
 },  
 {  
   "id": "a\_c184714",  
   "merchant": " netflix com ",  
   "amount": 357.64,  
   "ts": "2026-01-12 14:02:14 \+0530"  
 },  
 {  
   "id": "a\_t763829",  
   "merchant": "STARBUCKS 123",  
   "amount": 37.51,  
   "ts": "2026-01-12 12:30:11 \+0530"  
 },  
 {  
   "id": "a\_q409222",  
   "merchant": "Uber  Trip",  
   "amount": 193.55,  
   "ts": "2026-01-12 15:26:15 \+0530"  
 },  
 {  
   "id": "a\_w219971",  
   "merchant": "UBER, BV",  
   "amount": 217.95,  
   "ts": "2026-01-12 17:13:08 \+0530"  
 },  
 {  
   "id": "a\_n573489",  
   "merchant": "local grocery",  
   "amount": 361.82,  
   "ts": "2026-01-12 11:57:25 \+0530"  
 },  
 {  
   "id": "a\_m536196",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 172.77,  
   "ts": "2026-01-12 14:07:40 \+0530"  
 },  
 {  
   "id": "a\_i492483",  
   "merchant": "NETFLIX.COM",  
   "amount": 216.25,  
   "ts": "2026-01-12 16:03:14 \+0530"  
 },  
 {  
   "id": "a\_c197758",  
   "merchant": "Fuel Station 9",  
   "amount": 38.23,  
   "ts": "2026-01-12 17:34:15 \+0530"  
 },  
 {  
   "id": "a\_b714953",  
   "merchant": "Starbucks Store, 123",  
   "amount": 406.83,  
   "ts": "2026-01-12 15:03:51 \+0530"  
 },  
 {  
   "id": "a\_n470774",  
   "merchant": "Restaurant ABC",  
   "amount": 168.17,  
   "ts": "2026-01-12 11:06:45 \+0530"  
 },  
 {  
   "id": "a\_b401601",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 434.71,  
   "ts": "2026-01-12 16:33:47 \+0530"  
 },  
 {  
   "id": "a\_g262246",  
   "merchant": "Uber  Trip",  
   "amount": 56.28,  
   "ts": "2026-01-12 14:37:06 \+0530"  
 },  
 {  
   "id": "a\_r485415",  
   "merchant": "NETFLIX",  
   "amount": 424.4,  
   "ts": "2026-01-12 13:11:12 \+0530"  
 },  
 {  
   "id": "a\_n986209",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 289.18,  
   "ts": "2026-01-12 17:20:51 \+0530"  
 },  
 {  
   "id": "a\_r127529",  
   "merchant": "UBER, BV",  
   "amount": 308.72,  
   "ts": "2026-01-12 15:50:59 \+0530"  
 },  
 {  
   "id": "a\_w899444",  
   "merchant": "Uber  Trip",  
   "amount": 19.33,  
   "ts": "2026-01-12 12:29:14 \+0530"  
 },  
 {  
   "id": "a\_e693830",  
   "merchant": "Starbucks Store, 123",  
   "amount": 178.76,  
   "ts": "2026-01-12 11:39:04 \+0530"  
 },  
 {  
   "id": "a\_a196214",  
   "merchant": "Netflix.com",  
   "amount": 75.23,  
   "ts": "2026-01-12 15:45:47 \+0530"  
 },  
 {  
   "id": "a\_k265055",  
   "merchant": "NETFLIX",  
   "amount": 191.22,  
   "ts": "2026-01-12 14:07:45 \+0530"  
 },  
 {  
   "id": "a\_t189083",  
   "merchant": "Starbucks Store, 123",  
   "amount": 389.53,  
   "ts": "2026-01-12 15:09:58 \+0530"  
 },  
 {  
   "id": "a\_b806707",  
   "merchant": "LOCAL GROCERY",  
   "amount": 82.92,  
   "ts": "2026-01-12 15:37:25 \+0530"  
 },  
 {  
   "id": "a\_t563516",  
   "merchant": "Amazon   Marketplace",  
   "amount": 332.66,  
   "ts": "2026-01-12 14:25:14 \+0530"  
 },  
 {  
   "id": "a\_l550797",  
   "merchant": "STARBUCKS 123",  
   "amount": 378.7,  
   "ts": "2026-01-12 11:02:09 \+0530"  
 },  
 {  
   "id": "a\_q799539",  
   "merchant": "amzn mktplace",  
   "amount": 340.85,  
   "ts": "2026-01-12 14:25:45 \+0530"  
 },  
 {  
   "id": "a\_b108060",  
   "merchant": "STARBUCKS 123",  
   "amount": 200.66,  
   "ts": "2026-01-12 15:27:18 \+0530"  
 },  
 {  
   "id": "a\_i403507",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 384.84,  
   "ts": "2026-01-12 16:57:20 \+0530"  
 },  
 {  
   "id": "a\_f235461",  
   "merchant": "Starbucks Store 123",  
   "amount": 251.21,  
   "ts": "2026-01-12 13:55:11 \+0530"  
 },  
 {  
   "id": "a\_v945362",  
   "merchant": "STARBUCKS 123",  
   "amount": 252.63,  
   "ts": "2026-01-12 17:35:07 \+0530"  
 },  
 {  
   "id": "a\_n119651",  
   "merchant": " starbucks 123 ",  
   "amount": 431.57,  
   "ts": "2026-01-12 10:23:06 \+0530"  
 },  
 {  
   "id": "a\_n701253",  
   "merchant": "Uber Trip",  
   "amount": 431.74,  
   "ts": "2026-01-12 15:14:26 \+0530"  
 },  
 {  
   "id": "a\_a440544",  
   "merchant": " starbucks 123 ",  
   "amount": 148.31,  
   "ts": "2026-01-12 13:41:12 \+0530"  
 },  
 {  
   "id": "a\_l192410",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 232.75,  
   "ts": "2026-01-12 16:16:37 \+0530"  
 },  
 {  
   "id": "a\_m649414",  
   "merchant": "Starbucks Store 123",  
   "amount": 125.44,  
   "ts": "2026-01-12 15:21:36 \+0530"  
 },  
 {  
   "id": "a\_k916964",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 374.17,  
   "ts": "2026-01-12 12:01:00 \+0530"  
 },  
 {  
   "id": "a\_q303322",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 318.45,  
   "ts": "2026-01-12 14:49:46 \+0530"  
 },  
 {  
   "id": "a\_u954628",  
   "merchant": "Local-Grocery",  
   "amount": 178.79,  
   "ts": "2026-01-12 17:27:15 \+0530"  
 },  
 {  
   "id": "a\_f731661",  
   "merchant": "Amazon Marketplace",  
   "amount": 77.49,  
   "ts": "2026-01-12 11:47:44 \+0530"  
 },  
 {  
   "id": "a\_u618119",  
   "merchant": "amzn mktplace",  
   "amount": 42.3,  
   "ts": "2026-01-12 17:02:04 \+0530"  
 },  
 {  
   "id": "a\_v691912",  
   "merchant": "UBER, BV",  
   "amount": 381.14,  
   "ts": "2026-01-12 14:05:09 \+0530"  
 },  
 {  
   "id": "a\_k258290",  
   "merchant": "NETFLIX,COM",  
   "amount": 164.99,  
   "ts": "2026-01-12 15:42:32 \+0530"  
 },  
 {  
   "id": "a\_z388016",  
   "merchant": " starbucks 123 ",  
   "amount": 223.89,  
   "ts": "2026-01-12 12:45:22 \+0530"  
 },  
 {  
   "id": "a\_o573866",  
   "merchant": "Uber  Trip",  
   "amount": 256.13,  
   "ts": "2026-01-12 12:49:31 \+0530"  
 },  
 {  
   "id": "a\_u996840",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 416.69,  
   "ts": "2026-01-12 10:41:54 \+0530"  
 },  
 {  
   "id": "a\_s681242",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 256.0,  
   "ts": "2026-01-12 14:12:42 \+0530"  
 },  
 {  
   "id": "a\_s783501",  
   "merchant": "Coffee Shop 77",  
   "amount": 227.62,  
   "ts": "2026-01-12 17:22:16 \+0530"  
 },  
 {  
   "id": "a\_b572407",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 240.44,  
   "ts": "2026-01-12 11:22:23 \+0530"  
 },  
 {  
   "id": "a\_q777513",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 491.28,  
   "ts": "2026-01-12 10:46:03 \+0530"  
 },  
 {  
   "id": "a\_q648141",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 355.27,  
   "ts": "2026-01-12 13:59:58 \+0530"  
 },  
 {  
   "id": "a\_m528625",  
   "merchant": "Uber  Trip",  
   "amount": 189.56,  
   "ts": "2026-01-12 12:34:29 \+0530"  
 },  
 {  
   "id": "a\_u450905",  
   "merchant": "NETFLIX,COM",  
   "amount": 30.92,  
   "ts": "2026-01-12 15:44:32 \+0530"  
 },  
 {  
   "id": "a\_j364236",  
   "merchant": "Amazon Marketplace",  
   "amount": 281.19,  
   "ts": "2026-01-12 13:31:06 \+0530"  
 },  
 {  
   "id": "a\_c711046",  
   "merchant": "NETFLIX,COM",  
   "amount": 437.27,  
   "ts": "2026-01-12 13:02:03 \+0530"  
 },  
 {  
   "id": "a\_w794299",  
   "merchant": "NETFLIX.COM",  
   "amount": 158.56,  
   "ts": "2026-01-12 15:58:17 \+0530"  
 },  
 {  
   "id": "a\_j686072",  
   "merchant": "STARBUCKS.123",  
   "amount": 355.87,  
   "ts": "2026-01-12 10:46:16 \+0530"  
 },  
 {  
   "id": "a\_w969226",  
   "merchant": "Starbucks Store, 123",  
   "amount": 407.53,  
   "ts": "2026-01-12 16:05:58 \+0530"  
 },  
 {  
   "id": "a\_n633188",  
   "merchant": "Restaurant ABC",  
   "amount": 51.26,  
   "ts": "2026-01-12 16:06:09 \+0530"  
 },  
 {  
   "id": "a\_k903373",  
   "merchant": "Starbucks Store, 123",  
   "amount": 157.89,  
   "ts": "2026-01-12 11:56:58 \+0530"  
 },  
 {  
   "id": "a\_c410160",  
   "merchant": "UBER BV",  
   "amount": 499.99,  
   "ts": "2026-01-12 11:24:37 \+0530"  
 },  
 {  
   "id": "a\_x652108",  
   "merchant": "Local Grocery",  
   "amount": 256.31,  
   "ts": "2026-01-12 14:54:47 \+0530"  
 },  
 {  
   "id": "a\_e726258",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 438.81,  
   "ts": "2026-01-12 15:37:31 \+0530"  
 },  
 {  
   "id": "a\_y755087",  
   "merchant": "STARBUCKS 123",  
   "amount": 94.52,  
   "ts": "2026-01-12 16:18:23 \+0530"  
 },  
 {  
   "id": "a\_n481995",  
   "merchant": "LOCAL GROCERY",  
   "amount": 361.99,  
   "ts": "2026-01-12 10:23:51 \+0530"  
 },  
 {  
   "id": "a\_j888939",  
   "merchant": "NETFLIX",  
   "amount": 484.95,  
   "ts": "2026-01-12 15:33:27 \+0530"  
 },  
 {  
   "id": "a\_j947903",  
   "merchant": " netflix com ",  
   "amount": 120.83,  
   "ts": "2026-01-12 12:10:37 \+0530"  
 },  
 {  
   "id": "a\_o584106",  
   "merchant": "LOCAL GROCERY",  
   "amount": 187.08,  
   "ts": "2026-01-12 15:11:31 \+0530"  
 },  
 {  
   "id": "a\_f956572",  
   "merchant": " netflix com ",  
   "amount": 253.94,  
   "ts": "2026-01-12 13:48:38 \+0530"  
 },  
 {  
   "id": "a\_b772324",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 73.51,  
   "ts": "2026-01-12 12:16:32 \+0530"  
 },  
 {  
   "id": "a\_w986982",  
   "merchant": "Uber  Trip",  
   "amount": 279.5,  
   "ts": "2026-01-12 10:56:01 \+0530"  
 },  
 {  
   "id": "a\_f386042",  
   "merchant": "Uber Trip",  
   "amount": 146.11,  
   "ts": "2026-01-12 16:56:45 \+0530"  
 },  
 {  
   "id": "a\_c332823",  
   "merchant": "UBER, BV",  
   "amount": 77.94,  
   "ts": "2026-01-12 13:58:55 \+0530"  
 },  
 {  
   "id": "a\_b515712",  
   "merchant": "Local-Grocery",  
   "amount": 465.58,  
   "ts": "2026-01-12 13:46:35 \+0530"  
 },  
 {  
   "id": "a\_l335445",  
   "merchant": "UBER BV",  
   "amount": 196.15,  
   "ts": "2026-01-12 10:44:34 \+0530"  
 },  
 {  
   "id": "a\_k929822",  
   "merchant": "Amazon Marketplace",  
   "amount": 420.39,  
   "ts": "2026-01-12 15:54:57 \+0530"  
 },  
 {  
   "id": "a\_p829884",  
   "merchant": "Amazon Marketplace",  
   "amount": 147.01,  
   "ts": "2026-01-12 17:33:09 \+0530"  
 },  
 {  
   "id": "a\_c119927",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 227.06,  
   "ts": "2026-01-12 10:02:52 \+0530"  
 },  
 {  
   "id": "a\_t653408",  
   "merchant": "STARBUCKS 123",  
   "amount": 276.66,  
   "ts": "2026-01-12 16:37:10 \+0530"  
 },  
 {  
   "id": "a\_b350021",  
   "merchant": "Starbucks Store, 123",  
   "amount": 122.63,  
   "ts": "2026-01-12 11:06:32 \+0530"  
 },  
 {  
   "id": "a\_p725726",  
   "merchant": "STARBUCKS.123",  
   "amount": 231.27,  
   "ts": "2026-01-12 11:00:32 \+0530"  
 },  
 {  
   "id": "a\_e405420",  
   "merchant": "UBER, BV",  
   "amount": 289.65,  
   "ts": "2026-01-12 16:32:25 \+0530"  
 },  
 {  
   "id": "a\_n296412",  
   "merchant": "STARBUCKS.123",  
   "amount": 179.64,  
   "ts": "2026-01-12 15:11:39 \+0530"  
 },  
 {  
   "id": "a\_c651613",  
   "merchant": "Netflix.com",  
   "amount": 264.12,  
   "ts": "2026-01-12 13:16:56 \+0530"  
 },  
 {  
   "id": "a\_p145649",  
   "merchant": "UBER, BV",  
   "amount": 279.31,  
   "ts": "2026-01-12 13:33:14 \+0530"  
 },  
 {  
   "id": "a\_l926823",  
   "merchant": "NETFLIX.COM",  
   "amount": 130.56,  
   "ts": "2026-01-12 10:08:52 \+0530"  
 },  
 {  
   "id": "a\_d908977",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 367.81,  
   "ts": "2026-01-12 15:43:06 \+0530"  
 },  
 {  
   "id": "a\_r454992",  
   "merchant": "Uber  Trip",  
   "amount": 71.04,  
   "ts": "2026-01-12 13:12:24 \+0530"  
 },  
 {  
   "id": "a\_o829168",  
   "merchant": "LOCAL GROCERY",  
   "amount": 416.04,  
   "ts": "2026-01-12 16:13:55 \+0530"  
 },  
 {  
   "id": "a\_w913894",  
   "merchant": "UBER BV",  
   "amount": 406.69,  
   "ts": "2026-01-12 10:34:27 \+0530"  
 },  
 {  
   "id": "a\_z309159",  
   "merchant": "Coffee Shop 77",  
   "amount": 150.25,  
   "ts": "2026-01-12 10:23:55 \+0530"  
 },  
 {  
   "id": "a\_m955015",  
   "merchant": "Local-Grocery",  
   "amount": 467.02,  
   "ts": "2026-01-12 14:41:28 \+0530"  
 },  
 {  
   "id": "a\_g400009",  
   "merchant": "Uber  Trip",  
   "amount": 23.13,  
   "ts": "2026-01-12 15:53:17 \+0530"  
 },  
 {  
   "id": "a\_i230521",  
   "merchant": "Starbucks Store 123",  
   "amount": 433.57,  
   "ts": "2026-01-12 13:01:16 \+0530"  
 },  
 {  
   "id": "a\_o505426",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 445.3,  
   "ts": "2026-01-12 16:45:56 \+0530"  
 },  
 {  
   "id": "a\_l936020",  
   "merchant": "STARBUCKS 123",  
   "amount": 250.62,  
   "ts": "2026-01-12 14:31:42 \+0530"  
 },  
 {  
   "id": "a\_n731718",  
   "merchant": "Uber Trip",  
   "amount": 364.91,  
   "ts": "2026-01-12 10:43:06 \+0530"  
 },  
 {  
   "id": "a\_d183958",  
   "merchant": "Ecomm Seller X",  
   "amount": 275.01,  
   "ts": "2026-01-12 12:55:25 \+0530"  
 },  
 {  
   "id": "a\_w546815",  
   "merchant": "Starbucks Store, 123",  
   "amount": 156.72,  
   "ts": "2026-01-12 15:29:06 \+0530"  
 },  
 {  
   "id": "a\_x469685",  
   "merchant": "Amazon   Marketplace",  
   "amount": 179.02,  
   "ts": "2026-01-12 10:23:07 \+0530"  
 },  
 {  
   "id": "a\_b178686",  
   "merchant": "uber trip",  
   "amount": 140.9,  
   "ts": "2026-01-12 17:12:15 \+0530"  
 },  
 {  
   "id": "a\_x812413",  
   "merchant": " netflix com ",  
   "amount": 184.84,  
   "ts": "2026-01-12 17:17:30 \+0530"  
 },  
 {  
   "id": "a\_v919264",  
   "merchant": "Amazon Marketplace",  
   "amount": 75.68,  
   "ts": "2026-01-12 15:31:48 \+0530"  
 },  
 {  
   "id": "a\_u484123",  
   "merchant": "STARBUCKS 123",  
   "amount": 38.28,  
   "ts": "2026-01-12 17:05:10 \+0530"  
 },  
 {  
   "id": "a\_v571778",  
   "merchant": "STARBUCKS.123",  
   "amount": 21.03,  
   "ts": "2026-01-12 11:23:47 \+0530"  
 },  
 {  
   "id": "a\_s244383",  
   "merchant": "Pharmacy 22",  
   "amount": 224.75,  
   "ts": "2026-01-12 10:41:59 \+0530"  
 },  
 {  
   "id": "a\_h218959",  
   "merchant": "uber trip",  
   "amount": 160.62,  
   "ts": "2026-01-12 12:32:12 \+0530"  
 },  
 {  
   "id": "a\_r223481",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 252.03,  
   "ts": "2026-01-12 13:31:20 \+0530"  
 },  
 {  
   "id": "a\_t399449",  
   "merchant": "Starbucks Store, 123",  
   "amount": 353.5,  
   "ts": "2026-01-12 11:57:11 \+0530"  
 },  
 {  
   "id": "a\_c574001",  
   "merchant": " netflix com ",  
   "amount": 104.09,  
   "ts": "2026-01-12 11:14:07 \+0530"  
 },  
 {  
   "id": "a\_z815054",  
   "merchant": "amzn mktplace",  
   "amount": 225.33,  
   "ts": "2026-01-12 10:47:56 \+0530"  
 },  
 {  
   "id": "a\_c676713",  
   "merchant": "Pharmacy 22",  
   "amount": 335.58,  
   "ts": "2026-01-12 16:27:30 \+0530"  
 },  
 {  
   "id": "a\_w834233",  
   "merchant": "Uber  Trip",  
   "amount": 426.42,  
   "ts": "2026-01-12 16:28:44 \+0530"  
 },  
 {  
   "id": "a\_g930541",  
   "merchant": "NETFLIX.COM",  
   "amount": 256.83,  
   "ts": "2026-01-12 11:06:16 \+0530"  
 },  
 {  
   "id": "a\_s486831",  
   "merchant": "Amazon   Marketplace",  
   "amount": 18.01,  
   "ts": "2026-01-12 15:02:34 \+0530"  
 },  
 {  
   "id": "a\_c424314",  
   "merchant": "UBER, BV",  
   "amount": 69.24,  
   "ts": "2026-01-12 10:47:05 \+0530"  
 },  
 {  
   "id": "a\_n963333",  
   "merchant": " starbucks 123 ",  
   "amount": 265.22,  
   "ts": "2026-01-12 17:00:03 \+0530"  
 },  
 {  
   "id": "a\_c571832",  
   "merchant": "UBER BV",  
   "amount": 486.33,  
   "ts": "2026-01-12 15:50:48 \+0530"  
 },  
 {  
   "id": "a\_y678083",  
   "merchant": "UBER, BV",  
   "amount": 175.53,  
   "ts": "2026-01-12 17:59:28 \+0530"  
 },  
 {  
   "id": "a\_n627290",  
   "merchant": "NETFLIX",  
   "amount": 385.05,  
   "ts": "2026-01-12 11:10:28 \+0530"  
 },  
 {  
   "id": "a\_e418897",  
   "merchant": "Local Grocery",  
   "amount": 416.54,  
   "ts": "2026-01-12 14:42:57 \+0530"  
 },  
 {  
   "id": "a\_l644103",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 467.7,  
   "ts": "2026-01-12 12:03:08 \+0530"  
 },  
 {  
   "id": "a\_r387782",  
   "merchant": "Local Grocery",  
   "amount": 129.05,  
   "ts": "2026-01-12 15:46:49 \+0530"  
 },  
 {  
   "id": "a\_q772124",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 309.14,  
   "ts": "2026-01-12 10:51:01 \+0530"  
 },  
 {  
   "id": "a\_t855638",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 81.33,  
   "ts": "2026-01-12 15:59:45 \+0530"  
 },  
 {  
   "id": "a\_b965322",  
   "merchant": "Local-Grocery",  
   "amount": 422.05,  
   "ts": "2026-01-12 15:07:48 \+0530"  
 },  
 {  
   "id": "a\_y704788",  
   "merchant": "Local Grocery",  
   "amount": 27.5,  
   "ts": "2026-01-12 15:50:19 \+0530"  
 },  
 {  
   "id": "a\_t770030",  
   "merchant": "STARBUCKS 123",  
   "amount": 384.64,  
   "ts": "2026-01-12 13:47:30 \+0530"  
 },  
 {  
   "id": "a\_j606404",  
   "merchant": "amzn mktplace",  
   "amount": 275.06,  
   "ts": "2026-01-12 15:50:34 \+0530"  
 },  
 {  
   "id": "a\_w162823",  
   "merchant": " starbucks 123 ",  
   "amount": 152.23,  
   "ts": "2026-01-12 10:39:51 \+0530"  
 },  
 {  
   "id": "a\_k736129",  
   "merchant": "Amazon   Marketplace",  
   "amount": 244.7,  
   "ts": "2026-01-12 11:51:25 \+0530"  
 },  
 {  
   "id": "a\_l518084",  
   "merchant": "amzn mktplace",  
   "amount": 163.06,  
   "ts": "2026-01-12 17:50:05 \+0530"  
 },  
 {  
   "id": "a\_k353480",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 259.92,  
   "ts": "2026-01-12 10:57:59 \+0530"  
 },  
 {  
   "id": "a\_d153063",  
   "merchant": "Uber  Trip",  
   "amount": 227.55,  
   "ts": "2026-01-12 11:16:56 \+0530"  
 },  
 {  
   "id": "a\_h267457",  
   "merchant": " starbucks 123 ",  
   "amount": 432.76,  
   "ts": "2026-01-12 13:48:21 \+0530"  
 },  
 {  
   "id": "a\_y267082",  
   "merchant": "local grocery",  
   "amount": 361.96,  
   "ts": "2026-01-12 11:43:40 \+0530"  
 },  
 {  
   "id": "a\_p124302",  
   "merchant": "UBER, BV",  
   "amount": 236.07,  
   "ts": "2026-01-12 12:48:30 \+0530"  
 },  
 {  
   "id": "a\_g711680",  
   "merchant": "Amazon   Marketplace",  
   "amount": 255.2,  
   "ts": "2026-01-12 12:11:28 \+0530"  
 },  
 {  
   "id": "a\_u805188",  
   "merchant": "Starbucks Store 123",  
   "amount": 144.27,  
   "ts": "2026-01-12 15:26:19 \+0530"  
 },  
 {  
   "id": "a\_n240440",  
   "merchant": "UBER, BV",  
   "amount": 9.05,  
   "ts": "2026-01-12 10:58:42 \+0530"  
 },  
 {  
   "id": "a\_b520040",  
   "merchant": "Local-Grocery",  
   "amount": 383.11,  
   "ts": "2026-01-12 13:19:51 \+0530"  
 },  
 {  
   "id": "a\_j177103",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 101.35,  
   "ts": "2026-01-12 15:02:08 \+0530"  
 },  
 {  
   "id": "a\_t813449",  
   "merchant": " starbucks 123 ",  
   "amount": 383.38,  
   "ts": "2026-01-12 12:32:45 \+0530"  
 },  
 {  
   "id": "a\_l933510",  
   "merchant": "UBER BV",  
   "amount": 487.67,  
   "ts": "2026-01-12 13:35:07 \+0530"  
 },  
 {  
   "id": "a\_e308727",  
   "merchant": "Restaurant ABC",  
   "amount": 469.78,  
   "ts": "2026-01-12 16:52:18 \+0530"  
 },  
 {  
   "id": "a\_b243693",  
   "merchant": "uber trip",  
   "amount": 252.54,  
   "ts": "2026-01-12 10:24:44 \+0530"  
 },  
 {  
   "id": "a\_t640546",  
   "merchant": " netflix com ",  
   "amount": 262.07,  
   "ts": "2026-01-12 11:21:20 \+0530"  
 },  
 {  
   "id": "a\_t875232",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 162.67,  
   "ts": "2026-01-12 13:34:36 \+0530"  
 },  
 {  
   "id": "a\_q658198",  
   "merchant": "local grocery",  
   "amount": 171.5,  
   "ts": "2026-01-12 17:32:21 \+0530"  
 },  
 {  
   "id": "a\_a486251",  
   "merchant": "UBER, BV",  
   "amount": 153.4,  
   "ts": "2026-01-12 17:25:30 \+0530"  
 },  
 {  
   "id": "a\_j935457",  
   "merchant": "Starbucks Store 123",  
   "amount": 490.05,  
   "ts": "2026-01-12 15:18:46 \+0530"  
 },  
 {  
   "id": "a\_u920934",  
   "merchant": "Local Grocery",  
   "amount": 300.47,  
   "ts": "2026-01-12 12:25:00 \+0530"  
 },  
 {  
   "id": "a\_b711868",  
   "merchant": "Restaurant ABC",  
   "amount": 290.93,  
   "ts": "2026-01-12 16:33:58 \+0530"  
 },  
 {  
   "id": "a\_y982027",  
   "merchant": "UBER, BV",  
   "amount": 316.37,  
   "ts": "2026-01-12 15:38:27 \+0530"  
 },  
 {  
   "id": "a\_d300051",  
   "merchant": "STARBUCKS 123",  
   "amount": 20.63,  
   "ts": "2026-01-12 16:22:34 \+0530"  
 },  
 {  
   "id": "a\_w313795",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 212.3,  
   "ts": "2026-01-12 13:45:31 \+0530"  
 },  
 {  
   "id": "a\_x862736",  
   "merchant": "STARBUCKS.123",  
   "amount": 460.66,  
   "ts": "2026-01-12 17:56:40 \+0530"  
 },  
 {  
   "id": "a\_k794395",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 261.75,  
   "ts": "2026-01-12 15:06:19 \+0530"  
 },  
 {  
   "id": "a\_o659024",  
   "merchant": "uber trip",  
   "amount": 160.31,  
   "ts": "2026-01-12 11:34:34 \+0530"  
 },  
 {  
   "id": "a\_x813813",  
   "merchant": "Starbucks Store, 123",  
   "amount": 339.55,  
   "ts": "2026-01-12 17:53:51 \+0530"  
 },  
 {  
   "id": "a\_h392559",  
   "merchant": "NETFLIX.COM",  
   "amount": 306.99,  
   "ts": "2026-01-12 11:45:00 \+0530"  
 },  
 {  
   "id": "a\_y403042",  
   "merchant": "UBER BV",  
   "amount": 471.04,  
   "ts": "2026-01-12 12:42:32 \+0530"  
 },  
 {  
   "id": "a\_r934042",  
   "merchant": " netflix com ",  
   "amount": 161.94,  
   "ts": "2026-01-12 13:10:29 \+0530"  
 },  
 {  
   "id": "a\_r498138",  
   "merchant": "NETFLIX.COM",  
   "amount": 65.33,  
   "ts": "2026-01-12 16:09:31 \+0530"  
 },  
 {  
   "id": "a\_z253591",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 410.1,  
   "ts": "2026-01-12 17:02:11 \+0530"  
 },  
 {  
   "id": "a\_l563864",  
   "merchant": "Starbucks Store, 123",  
   "amount": 477.88,  
   "ts": "2026-01-12 10:43:09 \+0530"  
 },  
 {  
   "id": "a\_r384303",  
   "merchant": " netflix com ",  
   "amount": 110.92,  
   "ts": "2026-01-12 17:32:10 \+0530"  
 },  
 {  
   "id": "a\_x715336",  
   "merchant": "Pharmacy 22",  
   "amount": 72.95,  
   "ts": "2026-01-12 15:36:14 \+0530"  
 },  
 {  
   "id": "a\_h768933",  
   "merchant": "Starbucks Store 123",  
   "amount": 336.57,  
   "ts": "2026-01-12 14:50:01 \+0530"  
 },  
 {  
   "id": "a\_p205428",  
   "merchant": "Starbucks Store 123",  
   "amount": 209.58,  
   "ts": "2026-01-12 16:31:46 \+0530"  
 },  
 {  
   "id": "a\_f526673",  
   "merchant": "NETFLIX",  
   "amount": 7.56,  
   "ts": "2026-01-12 15:00:33 \+0530"  
 },  
 {  
   "id": "a\_y401198",  
   "merchant": " netflix com ",  
   "amount": 241.34,  
   "ts": "2026-01-12 11:48:55 \+0530"  
 },  
 {  
   "id": "a\_u702022",  
   "merchant": "Starbucks Store 123",  
   "amount": 454.43,  
   "ts": "2026-01-12 10:48:51 \+0530"  
 },  
 {  
   "id": "a\_n978292",  
   "merchant": "Starbucks Store 123",  
   "amount": 457.05,  
   "ts": "2026-01-12 11:35:36 \+0530"  
 },  
 {  
   "id": "a\_z619447",  
   "merchant": "Amazon Marketplace",  
   "amount": 498.6,  
   "ts": "2026-01-12 13:36:48 \+0530"  
 },  
 {  
   "id": "a\_b109717",  
   "merchant": "amzn mktplace",  
   "amount": 481.26,  
   "ts": "2026-01-12 12:38:05 \+0530"  
 },  
 {  
   "id": "a\_j576419",  
   "merchant": "STARBUCKS.123",  
   "amount": 58.11,  
   "ts": "2026-01-12 13:02:58 \+0530"  
 },  
 {  
   "id": "a\_q590755",  
   "merchant": "Restaurant ABC",  
   "amount": 264.54,  
   "ts": "2026-01-12 11:13:17 \+0530"  
 },  
 {  
   "id": "a\_o779776",  
   "merchant": "Starbucks Store 123",  
   "amount": 168.69,  
   "ts": "2026-01-12 16:39:17 \+0530"  
 },  
 {  
   "id": "a\_z409355",  
   "merchant": "STARBUCKS 123",  
   "amount": 11.97,  
   "ts": "2026-01-12 13:04:06 \+0530"  
 },  
 {  
   "id": "a\_u525187",  
   "merchant": "UBER BV",  
   "amount": 91.87,  
   "ts": "2026-01-12 17:47:34 \+0530"  
 },  
 {  
   "id": "a\_v200018",  
   "merchant": "local grocery",  
   "amount": 167.32,  
   "ts": "2026-01-12 13:38:54 \+0530"  
 },  
 {  
   "id": "a\_a373453",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 241.34,  
   "ts": "2026-01-12 12:15:28 \+0530"  
 },  
 {  
   "id": "a\_j710754",  
   "merchant": " starbucks 123 ",  
   "amount": 378.11,  
   "ts": "2026-01-12 13:00:01 \+0530"  
 },  
 {  
   "id": "a\_w347781",  
   "merchant": "Netflix.com",  
   "amount": 134.47,  
   "ts": "2026-01-12 13:16:21 \+0530"  
 },  
 {  
   "id": "a\_m819400",  
   "merchant": "Amazon Marketplace",  
   "amount": 235.37,  
   "ts": "2026-01-12 11:27:13 \+0530"  
 },  
 {  
   "id": "a\_j485397",  
   "merchant": "Uber  Trip",  
   "amount": 346.69,  
   "ts": "2026-01-12 15:48:49 \+0530"  
 },  
 {  
   "id": "a\_e577079",  
   "merchant": "UBER BV",  
   "amount": 478.89,  
   "ts": "2026-01-12 14:21:09 \+0530"  
 },  
 {  
   "id": "a\_r593699",  
   "merchant": "NETFLIX,COM",  
   "amount": 189.94,  
   "ts": "2026-01-12 16:23:21 \+0530"  
 },  
 {  
   "id": "a\_y724756",  
   "merchant": "NETFLIX",  
   "amount": 382.12,  
   "ts": "2026-01-12 16:11:33 \+0530"  
 },  
 {  
   "id": "a\_w479358",  
   "merchant": "Local Grocery",  
   "amount": 265.13,  
   "ts": "2026-01-12 14:48:27 \+0530"  
 },  
 {  
   "id": "a\_r630041",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 60.54,  
   "ts": "2026-01-12 17:32:37 \+0530"  
 },  
 {  
   "id": "a\_q563303",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 79.18,  
   "ts": "2026-01-12 12:59:11 \+0530"  
 },  
 {  
   "id": "a\_c635167",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 359.87,  
   "ts": "2026-01-12 14:26:57 \+0530"  
 },  
 {  
   "id": "a\_n578987",  
   "merchant": "Uber Trip",  
   "amount": 229.45,  
   "ts": "2026-01-12 14:40:18 \+0530"  
 },  
 {  
   "id": "a\_j858414",  
   "merchant": "UBER, BV",  
   "amount": 233.88,  
   "ts": "2026-01-12 17:20:41 \+0530"  
 },  
 {  
   "id": "a\_g706403",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 409.14,  
   "ts": "2026-01-12 16:47:18 \+0530"  
 },  
 {  
   "id": "a\_r163182",  
   "merchant": "Amazon   Marketplace",  
   "amount": 175.54,  
   "ts": "2026-01-12 10:34:49 \+0530"  
 },  
 {  
   "id": "a\_b401036",  
   "merchant": "Coffee Shop 77",  
   "amount": 493.14,  
   "ts": "2026-01-12 14:17:48 \+0530"  
 },  
 {  
   "id": "a\_u540781",  
   "merchant": "STARBUCKS 123",  
   "amount": 384.08,  
   "ts": "2026-01-12 16:38:17 \+0530"  
 },  
 {  
   "id": "a\_m184148",  
   "merchant": " starbucks 123 ",  
   "amount": 227.03,  
   "ts": "2026-01-12 13:26:09 \+0530"  
 },  
 {  
   "id": "a\_l224395",  
   "merchant": "NETFLIX,COM",  
   "amount": 70.83,  
   "ts": "2026-01-12 17:49:49 \+0530"  
 },  
 {  
   "id": "a\_x333507",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 199.56,  
   "ts": "2026-01-12 11:09:32 \+0530"  
 },  
 {  
   "id": "a\_v854024",  
   "merchant": "Local Grocery",  
   "amount": 489.35,  
   "ts": "2026-01-12 14:12:51 \+0530"  
 },  
 {  
   "id": "a\_h583000",  
   "merchant": "UBER, BV",  
   "amount": 192.74,  
   "ts": "2026-01-12 12:05:25 \+0530"  
 },  
 {  
   "id": "a\_x902674",  
   "merchant": "Starbucks Store, 123",  
   "amount": 98.3,  
   "ts": "2026-01-12 17:57:58 \+0530"  
 },  
 {  
   "id": "a\_a352356",  
   "merchant": "amzn mktplace",  
   "amount": 212.52,  
   "ts": "2026-01-12 16:58:22 \+0530"  
 },  
 {  
   "id": "a\_t804667",  
   "merchant": "Amazon Marketplace",  
   "amount": 299.0,  
   "ts": "2026-01-12 14:03:48 \+0530"  
 },  
 {  
   "id": "a\_m560285",  
   "merchant": "Netflix.com",  
   "amount": 126.14,  
   "ts": "2026-01-12 10:24:09 \+0530"  
 },  
 {  
   "id": "a\_b246942",  
   "merchant": "STARBUCKS 123",  
   "amount": 378.42,  
   "ts": "2026-01-12 17:03:50 \+0530"  
 },  
 {  
   "id": "a\_s434394",  
   "merchant": "Uber  Trip",  
   "amount": 120.91,  
   "ts": "2026-01-12 16:39:30 \+0530"  
 },  
 {  
   "id": "a\_e792282",  
   "merchant": "Uber  Trip",  
   "amount": 122.27,  
   "ts": "2026-01-12 17:58:33 \+0530"  
 },  
 {  
   "id": "a\_r720407",  
   "merchant": "uber trip",  
   "amount": 153.75,  
   "ts": "2026-01-12 10:33:17 \+0530"  
 },  
 {  
   "id": "a\_r619696",  
   "merchant": "LOCAL GROCERY",  
   "amount": 314.87,  
   "ts": "2026-01-12 13:53:18 \+0530"  
 },  
 {  
   "id": "a\_m923975",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 471.09,  
   "ts": "2026-01-12 16:06:38 \+0530"  
 },  
 {  
   "id": "a\_m292811",  
   "merchant": "uber trip",  
   "amount": 207.09,  
   "ts": "2026-01-12 12:43:46 \+0530"  
 },  
 {  
   "id": "a\_j999197",  
   "merchant": " netflix com ",  
   "amount": 124.28,  
   "ts": "2026-01-12 12:03:04 \+0530"  
 },  
 {  
   "id": "a\_n536740",  
   "merchant": " netflix com ",  
   "amount": 457.8,  
   "ts": "2026-01-12 15:07:08 \+0530"  
 },  
 {  
   "id": "a\_g446305",  
   "merchant": "UBER BV",  
   "amount": 197.27,  
   "ts": "2026-01-12 12:19:15 \+0530"  
 },  
 {  
   "id": "a\_r859782",  
   "merchant": " netflix com ",  
   "amount": 424.33,  
   "ts": "2026-01-12 10:50:29 \+0530"  
 },  
 {  
   "id": "a\_t732209",  
   "merchant": "Local Grocery",  
   "amount": 137.89,  
   "ts": "2026-01-12 16:07:54 \+0530"  
 },  
 {  
   "id": "a\_s859489",  
   "merchant": "Amazon Marketplace",  
   "amount": 98.14,  
   "ts": "2026-01-12 16:55:59 \+0530"  
 },  
 {  
   "id": "a\_k417915",  
   "merchant": "NETFLIX",  
   "amount": 242.3,  
   "ts": "2026-01-12 17:54:50 \+0530"  
 },  
 {  
   "id": "a\_x223014",  
   "merchant": "Coffee Shop 77",  
   "amount": 109.84,  
   "ts": "2026-01-12 11:43:46 \+0530"  
 },  
 {  
   "id": "a\_t837954",  
   "merchant": " netflix com ",  
   "amount": 457.65,  
   "ts": "2026-01-12 16:19:54 \+0530"  
 },  
 {  
   "id": "a\_j499152",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 278.28,  
   "ts": "2026-01-12 17:03:32 \+0530"  
 },  
 {  
   "id": "a\_q621433",  
   "merchant": "Uber  Trip",  
   "amount": 157.67,  
   "ts": "2026-01-12 13:16:24 \+0530"  
 },  
 {  
   "id": "a\_g488712",  
   "merchant": "uber trip",  
   "amount": 382.01,  
   "ts": "2026-01-12 12:54:53 \+0530"  
 },  
 {  
   "id": "a\_x253011",  
   "merchant": "Starbucks Store 123",  
   "amount": 283.45,  
   "ts": "2026-01-12 12:00:50 \+0530"  
 },  
 {  
   "id": "a\_x537862",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 467.71,  
   "ts": "2026-01-12 15:16:17 \+0530"  
 },  
 {  
   "id": "a\_s973844",  
   "merchant": "STARBUCKS 123",  
   "amount": 168.28,  
   "ts": "2026-01-12 13:27:26 \+0530"  
 },  
 {  
   "id": "a\_k369826",  
   "merchant": "STARBUCKS.123",  
   "amount": 329.03,  
   "ts": "2026-01-12 16:14:01 \+0530"  
 },  
 {  
   "id": "a\_x614426",  
   "merchant": " netflix com ",  
   "amount": 483.42,  
   "ts": "2026-01-12 15:50:27 \+0530"  
 },  
 {  
   "id": "a\_r612036",  
   "merchant": "Uber  Trip",  
   "amount": 88.79,  
   "ts": "2026-01-12 16:33:23 \+0530"  
 },  
 {  
   "id": "a\_q135466",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 495.21,  
   "ts": "2026-01-12 10:32:04 \+0530"  
 },  
 {  
   "id": "a\_b901467",  
   "merchant": "Local Grocery",  
   "amount": 472.87,  
   "ts": "2026-01-12 16:04:56 \+0530"  
 },  
 {  
   "id": "a\_c841521",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 422.06,  
   "ts": "2026-01-12 12:06:23 \+0530"  
 },  
 {  
   "id": "a\_b747847",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 255.34,  
   "ts": "2026-01-12 13:23:49 \+0530"  
 },  
 {  
   "id": "a\_p116495",  
   "merchant": "NETFLIX,COM",  
   "amount": 456.81,  
   "ts": "2026-01-12 16:00:03 \+0530"  
 },  
 {  
   "id": "a\_q857273",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 208.61,  
   "ts": "2026-01-12 10:09:08 \+0530"  
 },  
 {  
   "id": "a\_w806774",  
   "merchant": "Starbucks Store, 123",  
   "amount": 13.49,  
   "ts": "2026-01-12 17:24:05 \+0530"  
 },  
 {  
   "id": "a\_q256130",  
   "merchant": "STARBUCKS 123",  
   "amount": 57.97,  
   "ts": "2026-01-12 10:52:36 \+0530"  
 },  
 {  
   "id": "a\_l380310",  
   "merchant": "STARBUCKS.123",  
   "amount": 265.6,  
   "ts": "2026-01-12 17:25:12 \+0530"  
 },  
 {  
   "id": "a\_o692447",  
   "merchant": "Local Grocery",  
   "amount": 189.66,  
   "ts": "2026-01-12 13:41:44 \+0530"  
 },  
 {  
   "id": "a\_c785491",  
   "merchant": "STARBUCKS 123",  
   "amount": 153.49,  
   "ts": "2026-01-12 17:32:13 \+0530"  
 },  
 {  
   "id": "a\_m679549",  
   "merchant": "Coffee Shop 77",  
   "amount": 51.28,  
   "ts": "2026-01-12 13:27:07 \+0530"  
 },  
 {  
   "id": "a\_p986392",  
   "merchant": "Uber Trip",  
   "amount": 352.91,  
   "ts": "2026-01-12 10:45:09 \+0530"  
 },  
 {  
   "id": "a\_d653815",  
   "merchant": "Starbucks Store, 123",  
   "amount": 285.1,  
   "ts": "2026-01-12 17:48:33 \+0530"  
 },  
 {  
   "id": "a\_w694316",  
   "merchant": "LOCAL GROCERY",  
   "amount": 109.65,  
   "ts": "2026-01-12 17:54:06 \+0530"  
 },  
 {  
   "id": "a\_i669408",  
   "merchant": "Uber Trip",  
   "amount": 462.46,  
   "ts": "2026-01-12 16:13:47 \+0530"  
 },  
 {  
   "id": "a\_k116418",  
   "merchant": "Uber Trip",  
   "amount": 93.78,  
   "ts": "2026-01-12 17:44:26 \+0530"  
 },  
 {  
   "id": "a\_w960778",  
   "merchant": "Restaurant ABC",  
   "amount": 76.37,  
   "ts": "2026-01-12 17:28:12 \+0530"  
 },  
 {  
   "id": "a\_h694223",  
   "merchant": "Starbucks Store 123",  
   "amount": 153.14,  
   "ts": "2026-01-12 15:07:59 \+0530"  
 },  
 {  
   "id": "a\_k502491",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 340.13,  
   "ts": "2026-01-12 14:55:13 \+0530"  
 },  
 {  
   "id": "a\_x182237",  
   "merchant": "NETFLIX",  
   "amount": 396.3,  
   "ts": "2026-01-12 16:16:27 \+0530"  
 },  
 {  
   "id": "a\_h979414",  
   "merchant": "Uber  Trip",  
   "amount": 31.58,  
   "ts": "2026-01-12 13:58:42 \+0530"  
 },  
 {  
   "id": "a\_m911131",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 385.66,  
   "ts": "2026-01-12 15:25:16 \+0530"  
 },  
 {  
   "id": "a\_p339220",  
   "merchant": "Starbucks Store, 123",  
   "amount": 392.86,  
   "ts": "2026-01-12 14:06:03 \+0530"  
 },  
 {  
   "id": "a\_s792764",  
   "merchant": " starbucks 123 ",  
   "amount": 218.76,  
   "ts": "2026-01-12 16:11:12 \+0530"  
 },  
 {  
   "id": "a\_w177077",  
   "merchant": "STARBUCKS.123",  
   "amount": 151.21,  
   "ts": "2026-01-12 12:14:35 \+0530"  
 },  
 {  
   "id": "a\_u261771",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 193.1,  
   "ts": "2026-01-12 17:08:06 \+0530"  
 },  
 {  
   "id": "a\_a423241",  
   "merchant": "NETFLIX.COM",  
   "amount": 183.55,  
   "ts": "2026-01-12 10:49:58 \+0530"  
 },  
 {  
   "id": "a\_f551914",  
   "merchant": "Uber  Trip",  
   "amount": 55.6,  
   "ts": "2026-01-12 10:47:40 \+0530"  
 },  
 {  
   "id": "a\_a193972",  
   "merchant": "UBER, BV",  
   "amount": 258.66,  
   "ts": "2026-01-12 10:56:24 \+0530"  
 },  
 {  
   "id": "a\_k503951",  
   "merchant": "Starbucks Store 123",  
   "amount": 300.48,  
   "ts": "2026-01-12 17:08:24 \+0530"  
 },  
 {  
   "id": "a\_x686716",  
   "merchant": "Amazon   Marketplace",  
   "amount": 196.96,  
   "ts": "2026-01-12 10:46:15 \+0530"  
 },  
 {  
   "id": "a\_v499226",  
   "merchant": "LOCAL GROCERY",  
   "amount": 288.12,  
   "ts": "2026-01-12 11:32:36 \+0530"  
 },  
 {  
   "id": "a\_e166340",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 154.06,  
   "ts": "2026-01-12 14:29:18 \+0530"  
 },  
 {  
   "id": "a\_z180657",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 213.45,  
   "ts": "2026-01-12 14:24:27 \+0530"  
 },  
 {  
   "id": "a\_t746624",  
   "merchant": "STARBUCKS 123",  
   "amount": 361.85,  
   "ts": "2026-01-12 14:30:46 \+0530"  
 },  
 {  
   "id": "a\_m448354",  
   "merchant": "Amazon Marketplace",  
   "amount": 72.07,  
   "ts": "2026-01-12 10:03:41 \+0530"  
 },  
 {  
   "id": "a\_o450996",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 403.02,  
   "ts": "2026-01-12 12:59:58 \+0530"  
 },  
 {  
   "id": "a\_t829066",  
   "merchant": "STARBUCKS.123",  
   "amount": 72.38,  
   "ts": "2026-01-12 12:55:07 \+0530"  
 },  
 {  
   "id": "a\_dup\_p429445",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 452.9,  
   "ts": "2026-01-12 15:18:59 \+0530"  
 },  
 {  
   "id": "a\_dup\_m434640",  
   "merchant": "Uber Trip",  
   "amount": 230.43,  
   "ts": "2026-01-12 15:43:56 \+0530"  
 },  
 {  
   "id": "a\_dup\_x828716",  
   "merchant": "Amazon Marketplace",  
   "amount": 207.22,  
   "ts": "2026-01-12 16:16:29 \+0530"  
 },  
 {  
   "id": "a\_dup\_p703853",  
   "merchant": "Ecomm Seller X",  
   "amount": 275.01,  
   "ts": "2026-01-12 12:55:25 \+0530"  
 },  
 {  
   "id": "a\_dup\_h441716",  
   "merchant": "Starbucks Store 123",  
   "amount": 457.05,  
   "ts": "2026-01-12 11:35:36 \+0530"  
 },  
 {  
   "id": "a\_dup\_i956156",  
   "merchant": "UBER, BV",  
   "amount": 75.84,  
   "ts": "2026-01-12 14:11:50 \+0530"  
 },  
 {  
   "id": "a\_dup\_m482746",  
   "merchant": "amzn mktplace",  
   "amount": 481.26,  
   "ts": "2026-01-12 12:38:05 \+0530"  
 },  
 {  
   "id": "a\_dup\_s309505",  
   "merchant": "STARBUCKS.123",  
   "amount": 8.87,  
   "ts": "2026-01-12 17:26:14 \+0530"  
 },  
 {  
   "id": "a\_dup\_r288667",  
   "merchant": "Amazon Marketplace",  
   "amount": 147.01,  
   "ts": "2026-01-12 17:33:09 \+0530"  
 },  
 {  
   "id": "a\_dup\_y676820",  
   "merchant": "Local Grocery",  
   "amount": 129.05,  
   "ts": "2026-01-12 15:46:49 \+0530"  
 }  
\]

# input\_b.json

\[  
 {  
   "id": "b\_m407979",  
   "merchant": " netflix com ",  
   "amount": 372.3,  
   "ts": "2026-01-12 12:13:38 \+0530"  
 },  
 {  
   "id": "b\_q609288",  
   "merchant": "NETFLIX,COM",  
   "amount": 47.83,  
   "ts": "2026-01-12 13:51:35 \+0530"  
 },  
 {  
   "id": "b\_s591877",  
   "merchant": "amzn mktplace",  
   "amount": 255.36,  
   "ts": "2026-01-12 10:14:48 \+0530"  
 },  
 {  
   "id": "b\_k749667",  
   "merchant": "NETFLIX,COM",  
   "amount": 212.45,  
   "ts": "2026-01-12 14:04:15 \+0530"  
 },  
 {  
   "id": "b\_z126047",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 380.86,  
   "ts": "2026-01-12 11:27:07 \+0530"  
 },  
 {  
   "id": "b\_d887057",  
   "merchant": "STARBUCKS 123",  
   "amount": 111.59,  
   "ts": "2026-01-12 16:57:03 \+0530"  
 },  
 {  
   "id": "b\_t819373",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 182.69,  
   "ts": "2026-01-12 13:06:32 \+0530"  
 },  
 {  
   "id": "b\_w359427",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 270.4,  
   "ts": "2026-01-12 13:26:03 \+0530"  
 },  
 {  
   "id": "b\_d667296",  
   "merchant": "STARBUCKS.123",  
   "amount": 443.13,  
   "ts": "2026-01-12 13:16:41 \+0530"  
 },  
 {  
   "id": "b\_v510929",  
   "merchant": "NETFLIX,COM",  
   "amount": 332.52,  
   "ts": "2026-01-12 17:01:54 \+0530"  
 },  
 {  
   "id": "b\_f970933",  
   "merchant": "LOCAL GROCERY",  
   "amount": 193.24,  
   "ts": "2026-01-12 14:07:10 \+0530"  
 },  
 {  
   "id": "b\_f613681",  
   "merchant": "Starbucks Store, 123",  
   "amount": 180.68,  
   "ts": "2026-01-12 16:06:37 \+0530"  
 },  
 {  
   "id": "b\_p648004",  
   "merchant": "Coffee Shop 77",  
   "amount": 306.43,  
   "ts": "2026-01-12 11:33:48 \+0530"  
 },  
 {  
   "id": "b\_s868765",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 422.38,  
   "ts": "2026-01-12 17:04:04 \+0530"  
 },  
 {  
   "id": "b\_o583008",  
   "merchant": "Local-Grocery",  
   "amount": 203.6,  
   "ts": "2026-01-12 10:35:39 \+0530"  
 },  
 {  
   "id": "b\_l264348",  
   "merchant": "Local-Grocery",  
   "amount": 109.99,  
   "ts": "2026-01-12 14:32:17 \+0530"  
 },  
 {  
   "id": "b\_c804138",  
   "merchant": "Uber  Trip",  
   "amount": 73.95,  
   "ts": "2026-01-12 16:47:33 \+0530"  
 },  
 {  
   "id": "b\_k780285",  
   "merchant": "STARBUCKS.123",  
   "amount": 217.18,  
   "ts": "2026-01-12 15:18:27 \+0530"  
 },  
 {  
   "id": "b\_j492459",  
   "merchant": "Amazon   Marketplace",  
   "amount": 257.28,  
   "ts": "2026-01-12 10:49:29 \+0530"  
 },  
 {  
   "id": "b\_z227630",  
   "merchant": "LOCAL GROCERY",  
   "amount": 315.85,  
   "ts": "2026-01-12 17:12:13 \+0530"  
 },  
 {  
   "id": "b\_r939519",  
   "merchant": "uber trip",  
   "amount": 194.13,  
   "ts": "2026-01-12 14:14:55 \+0530"  
 },  
 {  
   "id": "b\_w844935",  
   "merchant": "Coffee Shop 77",  
   "amount": 341.64,  
   "ts": "2026-01-12 11:03:43 \+0530"  
 },  
 {  
   "id": "b\_x746329",  
   "merchant": "NETFLIX.COM",  
   "amount": 60.49,  
   "ts": "2026-01-12 13:56:32 \+0530"  
 },  
 {  
   "id": "b\_l443174",  
   "merchant": "amzn mktplace",  
   "amount": 438.78,  
   "ts": "2026-01-12 12:23:55 \+0530"  
 },  
 {  
   "id": "b\_near\_f819827",  
   "merchant": "amzn mktplace",  
   "amount": 438.84,  
   "ts": "2026-01-12 12:24:00 \+0530"  
 },  
 {  
   "id": "b\_c900928",  
   "merchant": "Amazon Marketplace",  
   "amount": 435.67,  
   "ts": "2026-01-12 12:42:26 \+0530"  
 },  
 {  
   "id": "b\_y528399",  
   "merchant": "UBER BV",  
   "amount": 190.33,  
   "ts": "2026-01-12 11:28:48 \+0530"  
 },  
 {  
   "id": "b\_s593056",  
   "merchant": "Coffee Shop 77",  
   "amount": 301.19,  
   "ts": "2026-01-12 14:27:53 \+0530"  
 },  
 {  
   "id": "b\_g145317",  
   "merchant": "Amazon   Marketplace",  
   "amount": 409.19,  
   "ts": "2026-01-12 16:54:55 \+0530"  
 },  
 {  
   "id": "b\_n380531",  
   "merchant": "UBER, BV",  
   "amount": 214.49,  
   "ts": "2026-01-12 11:56:13 \+0530"  
 },  
 {  
   "id": "b\_o556074",  
   "merchant": "NETFLIX",  
   "amount": 357.91,  
   "ts": "2026-01-12 13:38:02 \+0530"  
 },  
 {  
   "id": "b\_w348313",  
   "merchant": "STARBUCKS.123",  
   "amount": 228.25,  
   "ts": "2026-01-12 12:13:59 \+0530"  
 },  
 {  
   "id": "b\_g774497",  
   "merchant": "STARBUCKS.123",  
   "amount": 279.2,  
   "ts": "2026-01-12 15:20:36 \+0530"  
 },  
 {  
   "id": "b\_l237061",  
   "merchant": "amzn mktplace",  
   "amount": 34.13,  
   "ts": "2026-01-12 10:35:43 \+0530"  
 },  
 {  
   "id": "b\_near\_f480514",  
   "merchant": "amzn mktplace",  
   "amount": 34.19,  
   "ts": "2026-01-12 10:35:57 \+0530"  
 },  
 {  
   "id": "b\_b137736",  
   "merchant": "amzn mktplace",  
   "amount": 142.87,  
   "ts": "2026-01-12 14:25:08 \+0530"  
 },  
 {  
   "id": "b\_t640206",  
   "merchant": "LOCAL GROCERY",  
   "amount": 51.89,  
   "ts": "2026-01-12 16:00:31 \+0530"  
 },  
 {  
   "id": "b\_b379995",  
   "merchant": "starbucks 123",  
   "amount": 432.68,  
   "ts": "2026-01-12 10:30:43 \+0530"  
 },  
 {  
   "id": "b\_s369478",  
   "merchant": "Coffee Shop 77",  
   "amount": 34.95,  
   "ts": "2026-01-12 16:38:37 \+0530"  
 },  
 {  
   "id": "b\_y212669",  
   "merchant": "Starbucks Store, 123",  
   "amount": 234.15,  
   "ts": "2026-01-12 17:58:49 \+0530"  
 },  
 {  
   "id": "b\_j980597",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 463.49,  
   "ts": "2026-01-12 17:42:30 \+0530"  
 },  
 {  
   "id": "b\_d627731",  
   "merchant": "starbucks 123",  
   "amount": 111.05,  
   "ts": "2026-01-12 13:40:27 \+0530"  
 },  
 {  
   "id": "b\_i429593",  
   "merchant": "starbucks 123",  
   "amount": 136.5,  
   "ts": "2026-01-12 17:07:25 \+0530"  
 },  
 {  
   "id": "b\_j245075",  
   "merchant": "STARBUCKS.123",  
   "amount": 332.69,  
   "ts": "2026-01-12 14:26:12 \+0530"  
 },  
 {  
   "id": "b\_m846219",  
   "merchant": " starbucks 123 ",  
   "amount": 294.41,  
   "ts": "2026-01-12 14:34:28 \+0530"  
 },  
 {  
   "id": "b\_w235954",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 44.66,  
   "ts": "2026-01-12 11:42:28 \+0530"  
 },  
 {  
   "id": "b\_w513710",  
   "merchant": "UBER, BV",  
   "amount": 45.5,  
   "ts": "2026-01-12 15:59:21 \+0530"  
 },  
 {  
   "id": "b\_m219368",  
   "merchant": "Uber  Trip",  
   "amount": 106.17,  
   "ts": "2026-01-12 16:31:10 \+0530"  
 },  
 {  
   "id": "b\_a477372",  
   "merchant": "STARBUCKS.123",  
   "amount": 337.29,  
   "ts": "2026-01-12 12:43:25 \+0530"  
 },  
 {  
   "id": "b\_s922329",  
   "merchant": "Coffee Shop 77",  
   "amount": 9.53,  
   "ts": "2026-01-12 15:38:08 \+0530"  
 },  
 {  
   "id": "b\_o248455",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 255.64,  
   "ts": "2026-01-12 11:12:23 \+0530"  
 },  
 {  
   "id": "b\_b335333",  
   "merchant": "Local-Grocery",  
   "amount": 146.11,  
   "ts": "2026-01-12 13:58:11 \+0530"  
 },  
 {  
   "id": "b\_u701002",  
   "merchant": "STARBUCKS.123",  
   "amount": 8.79,  
   "ts": "2026-01-12 17:27:42 \+0530"  
 },  
 {  
   "id": "b\_w833400",  
   "merchant": "Coffee Shop 77",  
   "amount": 469.8,  
   "ts": "2026-01-12 11:14:37 \+0530"  
 },  
 {  
   "id": "b\_k291040",  
   "merchant": "NETFLIX.COM",  
   "amount": 345.19,  
   "ts": "2026-01-12 17:45:32 \+0530"  
 },  
 {  
   "id": "b\_r379267",  
   "merchant": "Uber  Trip",  
   "amount": 50.51,  
   "ts": "2026-01-12 13:49:55 \+0530"  
 },  
 {  
   "id": "b\_k563995",  
   "merchant": "uber trip",  
   "amount": 282.41,  
   "ts": "2026-01-12 11:00:51 \+0530"  
 },  
 {  
   "id": "b\_near\_q973061",  
   "merchant": "uber trip",  
   "amount": 282.35,  
   "ts": "2026-01-12 11:01:13 \+0530"  
 },  
 {  
   "id": "b\_d490570",  
   "merchant": "local grocery",  
   "amount": 22.93,  
   "ts": "2026-01-12 13:22:11 \+0530"  
 },  
 {  
   "id": "b\_h765578",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 25.42,  
   "ts": "2026-01-12 13:18:00 \+0530"  
 },  
 {  
   "id": "b\_q498377",  
   "merchant": "Local Grocery",  
   "amount": 342.38,  
   "ts": "2026-01-12 16:05:35 \+0530"  
 },  
 {  
   "id": "b\_l533318",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 487.21,  
   "ts": "2026-01-12 16:48:44 \+0530"  
 },  
 {  
   "id": "b\_m140044",  
   "merchant": "local grocery",  
   "amount": 441.14,  
   "ts": "2026-01-12 10:14:47 \+0530"  
 },  
 {  
   "id": "b\_o677673",  
   "merchant": "Fuel Station 9",  
   "amount": 402.05,  
   "ts": "2026-01-12 17:50:44 \+0530"  
 },  
 {  
   "id": "b\_q885226",  
   "merchant": "Starbucks Store 123",  
   "amount": 394.82,  
   "ts": "2026-01-12 10:58:00 \+0530"  
 },  
 {  
   "id": "b\_k997678",  
   "merchant": "Local Grocery",  
   "amount": 103.98,  
   "ts": "2026-01-12 14:10:57 \+0530"  
 },  
 {  
   "id": "b\_b180934",  
   "merchant": "local grocery",  
   "amount": 115.14,  
   "ts": "2026-01-12 15:59:10 \+0530"  
 },  
 {  
   "id": "b\_c612128",  
   "merchant": "STARBUCKS 123",  
   "amount": 483.69,  
   "ts": "2026-01-12 12:32:50 \+0530"  
 },  
 {  
   "id": "b\_h530366",  
   "merchant": "UBER, BV",  
   "amount": 168.73,  
   "ts": "2026-01-12 10:16:17 \+0530"  
 },  
 {  
   "id": "b\_g202583",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 24.14,  
   "ts": "2026-01-12 15:24:47 \+0530"  
 },  
 {  
   "id": "b\_d542010",  
   "merchant": "NETFLIX.COM",  
   "amount": 220.85,  
   "ts": "2026-01-12 14:38:28 \+0530"  
 },  
 {  
   "id": "b\_g903653",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 130.96,  
   "ts": "2026-01-12 16:28:29 \+0530"  
 },  
 {  
   "id": "b\_near\_d121015",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 130.95,  
   "ts": "2026-01-12 16:28:44 \+0530"  
 },  
 {  
   "id": "b\_g881285",  
   "merchant": "UBER, BV",  
   "amount": 344.7,  
   "ts": "2026-01-12 16:45:05 \+0530"  
 },  
 {  
   "id": "b\_e782149",  
   "merchant": "Amazon   Marketplace",  
   "amount": 39.39,  
   "ts": "2026-01-12 16:03:34 \+0530"  
 },  
 {  
   "id": "b\_i415948",  
   "merchant": "STARBUCKS 123",  
   "amount": 361.36,  
   "ts": "2026-01-12 12:43:10 \+0530"  
 },  
 {  
   "id": "b\_z238429",  
   "merchant": "amzn mktplace",  
   "amount": 470.66,  
   "ts": "2026-01-12 16:11:07 \+0530"  
 },  
 {  
   "id": "b\_l781826",  
   "merchant": "Uber  Trip",  
   "amount": 205.91,  
   "ts": "2026-01-12 17:34:00 \+0530"  
 },  
 {  
   "id": "b\_w753694",  
   "merchant": "starbucks 123",  
   "amount": 394.11,  
   "ts": "2026-01-12 15:31:14 \+0530"  
 },  
 {  
   "id": "b\_r753319",  
   "merchant": "uber trip",  
   "amount": 339.4,  
   "ts": "2026-01-12 14:38:54 \+0530"  
 },  
 {  
   "id": "b\_y633609",  
   "merchant": "Local-Grocery",  
   "amount": 331.02,  
   "ts": "2026-01-12 12:35:50 \+0530"  
 },  
 {  
   "id": "b\_f534926",  
   "merchant": "NETFLIX.COM",  
   "amount": 51.3,  
   "ts": "2026-01-12 16:51:15 \+0530"  
 },  
 {  
   "id": "b\_y409046",  
   "merchant": "Starbucks Store 123",  
   "amount": 78.01,  
   "ts": "2026-01-12 10:24:54 \+0530"  
 },  
 {  
   "id": "b\_x554025",  
   "merchant": "uber trip",  
   "amount": 230.14,  
   "ts": "2026-01-12 15:42:40 \+0530"  
 },  
 {  
   "id": "b\_k269127",  
   "merchant": "netflix com",  
   "amount": 249.74,  
   "ts": "2026-01-12 12:13:17 \+0530"  
 },  
 {  
   "id": "b\_z730314",  
   "merchant": "Restaurant ABC",  
   "amount": 234.86,  
   "ts": "2026-01-12 15:05:08 \+0530"  
 },  
 {  
   "id": "b\_l353674",  
   "merchant": "uber trip",  
   "amount": 308.38,  
   "ts": "2026-01-12 16:33:10 \+0530"  
 },  
 {  
   "id": "b\_a922010",  
   "merchant": "local grocery",  
   "amount": 225.79,  
   "ts": "2026-01-12 11:26:06 \+0530"  
 },  
 {  
   "id": "b\_m782539",  
   "merchant": "Uber Trip",  
   "amount": 420.76,  
   "ts": "2026-01-12 12:31:55 \+0530"  
 },  
 {  
   "id": "b\_m573038",  
   "merchant": "UBER BV",  
   "amount": 141.04,  
   "ts": "2026-01-12 10:40:48 \+0530"  
 },  
 {  
   "id": "b\_a195592",  
   "merchant": "Starbucks Store, 123",  
   "amount": 163.03,  
   "ts": "2026-01-12 14:56:27 \+0530"  
 },  
 {  
   "id": "b\_y244911",  
   "merchant": "Amazon   Marketplace",  
   "amount": 348.77,  
   "ts": "2026-01-12 16:26:22 \+0530"  
 },  
 {  
   "id": "b\_d838673",  
   "merchant": "Starbucks Store, 123",  
   "amount": 273.69,  
   "ts": "2026-01-12 13:48:17 \+0530"  
 },  
 {  
   "id": "b\_f651117",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 452.77,  
   "ts": "2026-01-12 15:18:18 \+0530"  
 },  
 {  
   "id": "b\_r326818",  
   "merchant": "local grocery",  
   "amount": 193.46,  
   "ts": "2026-01-12 10:02:34 \+0530"  
 },  
 {  
   "id": "b\_h360843",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 335.96,  
   "ts": "2026-01-12 17:15:12 \+0530"  
 },  
 {  
   "id": "b\_i795117",  
   "merchant": "Amazon   Marketplace",  
   "amount": 460.22,  
   "ts": "2026-01-12 15:39:15 \+0530"  
 },  
 {  
   "id": "b\_y921421",  
   "merchant": "starbucks 123",  
   "amount": 94.71,  
   "ts": "2026-01-12 12:21:53 \+0530"  
 },  
 {  
   "id": "b\_e414709",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 171.78,  
   "ts": "2026-01-12 13:25:55 \+0530"  
 },  
 {  
   "id": "b\_d694832",  
   "merchant": "Fuel Station 9",  
   "amount": 130.16,  
   "ts": "2026-01-12 10:45:39 \+0530"  
 },  
 {  
   "id": "b\_c796260",  
   "merchant": " netflix com ",  
   "amount": 499.67,  
   "ts": "2026-01-12 13:11:20 \+0530"  
 },  
 {  
   "id": "b\_w260282",  
   "merchant": "local grocery",  
   "amount": 80.41,  
   "ts": "2026-01-12 11:08:35 \+0530"  
 },  
 {  
   "id": "b\_j788607",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 235.01,  
   "ts": "2026-01-12 12:19:06 \+0530"  
 },  
 {  
   "id": "b\_h509489",  
   "merchant": "Local-Grocery",  
   "amount": 58.42,  
   "ts": "2026-01-12 10:15:18 \+0530"  
 },  
 {  
   "id": "b\_u895469",  
   "merchant": "uber trip",  
   "amount": 55.22,  
   "ts": "2026-01-12 17:02:22 \+0530"  
 },  
 {  
   "id": "b\_u607791",  
   "merchant": "starbucks 123",  
   "amount": 332.49,  
   "ts": "2026-01-12 10:36:18 \+0530"  
 },  
 {  
   "id": "b\_z441689",  
   "merchant": "starbucks 123",  
   "amount": 411.84,  
   "ts": "2026-01-12 10:59:06 \+0530"  
 },  
 {  
   "id": "b\_c863896",  
   "merchant": "netflix com",  
   "amount": 355.31,  
   "ts": "2026-01-12 13:56:28 \+0530"  
 },  
 {  
   "id": "b\_s752416",  
   "merchant": "Uber  Trip",  
   "amount": 309.99,  
   "ts": "2026-01-12 14:54:44 \+0530"  
 },  
 {  
   "id": "b\_p771531",  
   "merchant": "UBER, BV",  
   "amount": 137.94,  
   "ts": "2026-01-12 17:44:58 \+0530"  
 },  
 {  
   "id": "b\_a737869",  
   "merchant": "Pharmacy 22",  
   "amount": 441.26,  
   "ts": "2026-01-12 12:13:57 \+0530"  
 },  
 {  
   "id": "b\_near\_h645380",  
   "merchant": "Pharmacy 22",  
   "amount": 441.24,  
   "ts": "2026-01-12 12:13:35 \+0530"  
 },  
 {  
   "id": "b\_m120297",  
   "merchant": "uber trip",  
   "amount": 171.25,  
   "ts": "2026-01-12 14:30:02 \+0530"  
 },  
 {  
   "id": "b\_w151233",  
   "merchant": "NETFLIX.COM",  
   "amount": 280.41,  
   "ts": "2026-01-12 14:42:40 \+0530"  
 },  
 {  
   "id": "b\_p185327",  
   "merchant": "starbucks 123",  
   "amount": 246.96,  
   "ts": "2026-01-12 16:53:50 \+0530"  
 },  
 {  
   "id": "b\_o241219",  
   "merchant": "uber trip",  
   "amount": 227.06,  
   "ts": "2026-01-12 10:08:23 \+0530"  
 },  
 {  
   "id": "b\_i716134",  
   "merchant": "STARBUCKS.123",  
   "amount": 156.45,  
   "ts": "2026-01-12 15:16:35 \+0530"  
 },  
 {  
   "id": "b\_r672100",  
   "merchant": "Uber  Trip",  
   "amount": 215.4,  
   "ts": "2026-01-12 16:46:16 \+0530"  
 },  
 {  
   "id": "b\_m519721",  
   "merchant": "starbucks 123",  
   "amount": 139.2,  
   "ts": "2026-01-12 12:16:16 \+0530"  
 },  
 {  
   "id": "b\_l932975",  
   "merchant": "NETFLIX.COM",  
   "amount": 64.41,  
   "ts": "2026-01-12 14:52:19 \+0530"  
 },  
 {  
   "id": "b\_v622014",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 370.75,  
   "ts": "2026-01-12 12:31:58 \+0530"  
 },  
 {  
   "id": "b\_z329587",  
   "merchant": "Restaurant ABC",  
   "amount": 300.55,  
   "ts": "2026-01-12 10:55:38 \+0530"  
 },  
 {  
   "id": "b\_z935286",  
   "merchant": "Starbucks Store, 123",  
   "amount": 154.71,  
   "ts": "2026-01-12 16:27:07 \+0530"  
 },  
 {  
   "id": "b\_g210302",  
   "merchant": "Starbucks Store, 123",  
   "amount": 278.9,  
   "ts": "2026-01-12 16:21:31 \+0530"  
 },  
 {  
   "id": "b\_l295771",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 55.88,  
   "ts": "2026-01-12 10:06:21 \+0530"  
 },  
 {  
   "id": "b\_d493913",  
   "merchant": "uber trip",  
   "amount": 173.43,  
   "ts": "2026-01-12 10:28:20 \+0530"  
 },  
 {  
   "id": "b\_q914642",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 203.45,  
   "ts": "2026-01-12 10:40:29 \+0530"  
 },  
 {  
   "id": "b\_e550412",  
   "merchant": "NETFLIX,COM",  
   "amount": 79.05,  
   "ts": "2026-01-12 15:06:03 \+0530"  
 },  
 {  
   "id": "b\_r147144",  
   "merchant": "Coffee Shop 77",  
   "amount": 281.53,  
   "ts": "2026-01-12 13:47:26 \+0530"  
 },  
 {  
   "id": "b\_u111089",  
   "merchant": "Local-Grocery",  
   "amount": 388.87,  
   "ts": "2026-01-12 13:28:48 \+0530"  
 },  
 {  
   "id": "b\_f545348",  
   "merchant": "STARBUCKS.123",  
   "amount": 495.09,  
   "ts": "2026-01-12 12:47:23 \+0530"  
 },  
 {  
   "id": "b\_o338999",  
   "merchant": "amzn mktplace",  
   "amount": 54.07,  
   "ts": "2026-01-12 16:56:28 \+0530"  
 },  
 {  
   "id": "b\_a652127",  
   "merchant": "amzn mktplace",  
   "amount": 45.41,  
   "ts": "2026-01-12 12:11:58 \+0530"  
 },  
 {  
   "id": "b\_c349892",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 207.48,  
   "ts": "2026-01-12 16:16:41 \+0530"  
 },  
 {  
   "id": "b\_d627011",  
   "merchant": "STARBUCKS 123",  
   "amount": 147.69,  
   "ts": "2026-01-12 12:33:13 \+0530"  
 },  
 {  
   "id": "b\_c907511",  
   "merchant": "Uber  Trip",  
   "amount": 215.27,  
   "ts": "2026-01-12 14:56:59 \+0530"  
 },  
 {  
   "id": "b\_y240674",  
   "merchant": "local grocery",  
   "amount": 375.72,  
   "ts": "2026-01-12 15:09:23 \+0530"  
 },  
 {  
   "id": "b\_s240939",  
   "merchant": "amzn mktplace",  
   "amount": 155.29,  
   "ts": "2026-01-12 13:41:10 \+0530"  
 },  
 {  
   "id": "b\_b415955",  
   "merchant": "uber trip",  
   "amount": 44.88,  
   "ts": "2026-01-12 10:22:21 \+0530"  
 },  
 {  
   "id": "b\_q908881",  
   "merchant": "Starbucks Store, 123",  
   "amount": 17.85,  
   "ts": "2026-01-12 12:04:15 \+0530"  
 },  
 {  
   "id": "b\_k991003",  
   "merchant": "local grocery",  
   "amount": 475.62,  
   "ts": "2026-01-12 16:56:35 \+0530"  
 },  
 {  
   "id": "b\_w130603",  
   "merchant": "Local Grocery",  
   "amount": 237.39,  
   "ts": "2026-01-12 13:41:39 \+0530"  
 },  
 {  
   "id": "b\_w912164",  
   "merchant": " netflix com ",  
   "amount": 168.09,  
   "ts": "2026-01-12 16:18:53 \+0530"  
 },  
 {  
   "id": "b\_e589871",  
   "merchant": "netflix com",  
   "amount": 407.93,  
   "ts": "2026-01-12 14:59:52 \+0530"  
 },  
 {  
   "id": "b\_j708806",  
   "merchant": "local grocery",  
   "amount": 330.36,  
   "ts": "2026-01-12 14:56:51 \+0530"  
 },  
 {  
   "id": "b\_d980072",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 184.33,  
   "ts": "2026-01-12 16:54:34 \+0530"  
 },  
 {  
   "id": "b\_f120613",  
   "merchant": "Uber Trip",  
   "amount": 105.9,  
   "ts": "2026-01-12 14:59:10 \+0530"  
 },  
 {  
   "id": "b\_i169719",  
   "merchant": "starbucks 123",  
   "amount": 65.03,  
   "ts": "2026-01-12 15:43:16 \+0530"  
 },  
 {  
   "id": "b\_o463276",  
   "merchant": " starbucks 123 ",  
   "amount": 158.76,  
   "ts": "2026-01-12 10:06:23 \+0530"  
 },  
 {  
   "id": "b\_y740085",  
   "merchant": "amzn mktplace",  
   "amount": 233.24,  
   "ts": "2026-01-12 11:03:02 \+0530"  
 },  
 {  
   "id": "b\_o897361",  
   "merchant": "amzn mktplace",  
   "amount": 149.19,  
   "ts": "2026-01-12 16:26:03 \+0530"  
 },  
 {  
   "id": "b\_z380353",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 125.6,  
   "ts": "2026-01-12 15:00:50 \+0530"  
 },  
 {  
   "id": "b\_s629728",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 256.73,  
   "ts": "2026-01-12 11:15:30 \+0530"  
 },  
 {  
   "id": "b\_j382943",  
   "merchant": "Local-Grocery",  
   "amount": 364.18,  
   "ts": "2026-01-12 17:36:55 \+0530"  
 },  
 {  
   "id": "b\_s917506",  
   "merchant": "Fuel Station 9",  
   "amount": 433.45,  
   "ts": "2026-01-12 14:04:07 \+0530"  
 },  
 {  
   "id": "b\_y225653",  
   "merchant": "STARBUCKS.123",  
   "amount": 382.42,  
   "ts": "2026-01-12 13:25:36 \+0530"  
 },  
 {  
   "id": "b\_y867747",  
   "merchant": "local grocery",  
   "amount": 287.75,  
   "ts": "2026-01-12 12:07:39 \+0530"  
 },  
 {  
   "id": "b\_b198400",  
   "merchant": "amzn mktplace",  
   "amount": 239.34,  
   "ts": "2026-01-12 17:23:19 \+0530"  
 },  
 {  
   "id": "b\_n423950",  
   "merchant": "Ecomm Seller X",  
   "amount": 250.19,  
   "ts": "2026-01-12 10:20:20 \+0530"  
 },  
 {  
   "id": "b\_t775586",  
   "merchant": "Pharmacy 22",  
   "amount": 435.46,  
   "ts": "2026-01-12 17:56:40 \+0530"  
 },  
 {  
   "id": "b\_b569228",  
   "merchant": "uber trip",  
   "amount": 12.54,  
   "ts": "2026-01-12 11:18:01 \+0530"  
 },  
 {  
   "id": "b\_s325683",  
   "merchant": "Coffee Shop 77",  
   "amount": 237.25,  
   "ts": "2026-01-12 12:23:59 \+0530"  
 },  
 {  
   "id": "b\_t205823",  
   "merchant": "Starbucks Store, 123",  
   "amount": 315.36,  
   "ts": "2026-01-12 16:54:04 \+0530"  
 },  
 {  
   "id": "b\_q518566",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 121.41,  
   "ts": "2026-01-12 16:15:13 \+0530"  
 },  
 {  
   "id": "b\_r930009",  
   "merchant": "netflix com",  
   "amount": 489.57,  
   "ts": "2026-01-12 16:55:36 \+0530"  
 },  
 {  
   "id": "b\_q734266",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 87.37,  
   "ts": "2026-01-12 12:42:57 \+0530"  
 },  
 {  
   "id": "b\_d132452",  
   "merchant": "Starbucks Store, 123",  
   "amount": 150.34,  
   "ts": "2026-01-12 13:23:38 \+0530"  
 },  
 {  
   "id": "b\_u444427",  
   "merchant": "Amazon Marketplace",  
   "amount": 435.9,  
   "ts": "2026-01-12 15:37:40 \+0530"  
 },  
 {  
   "id": "b\_m896000",  
   "merchant": "UBER, BV",  
   "amount": 75.71,  
   "ts": "2026-01-12 14:11:18 \+0530"  
 },  
 {  
   "id": "b\_n154551",  
   "merchant": "uber trip",  
   "amount": 449.7,  
   "ts": "2026-01-12 14:14:08 \+0530"  
 },  
 {  
   "id": "b\_y765911",  
   "merchant": "UBER, BV",  
   "amount": 223.92,  
   "ts": "2026-01-12 13:08:24 \+0530"  
 },  
 {  
   "id": "b\_e209442",  
   "merchant": "netflix com",  
   "amount": 346.8,  
   "ts": "2026-01-12 14:09:38 \+0530"  
 },  
 {  
   "id": "b\_m468431",  
   "merchant": "Fuel Station 9",  
   "amount": 57.51,  
   "ts": "2026-01-12 13:27:07 \+0530"  
 },  
 {  
   "id": "b\_g166232",  
   "merchant": "Uber  Trip",  
   "amount": 16.09,  
   "ts": "2026-01-12 17:32:55 \+0530"  
 },  
 {  
   "id": "b\_e783344",  
   "merchant": "STARBUCKS.123",  
   "amount": 433.24,  
   "ts": "2026-01-12 17:04:30 \+0530"  
 },  
 {  
   "id": "b\_e985300",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 417.55,  
   "ts": "2026-01-12 12:35:38 \+0530"  
 },  
 {  
   "id": "b\_z171615",  
   "merchant": "Starbucks Store, 123",  
   "amount": 113.6,  
   "ts": "2026-01-12 11:44:02 \+0530"  
 },  
 {  
   "id": "b\_f296757",  
   "merchant": "starbucks 123",  
   "amount": 21.5,  
   "ts": "2026-01-12 13:18:41 \+0530"  
 },  
 {  
   "id": "b\_p650209",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 166.47,  
   "ts": "2026-01-12 13:48:23 \+0530"  
 },  
 {  
   "id": "b\_d388831",  
   "merchant": "Amazon   Marketplace",  
   "amount": 438.9,  
   "ts": "2026-01-12 13:20:15 \+0530"  
 },  
 {  
   "id": "b\_o450527",  
   "merchant": "local grocery",  
   "amount": 132.29,  
   "ts": "2026-01-12 17:30:45 \+0530"  
 },  
 {  
   "id": "b\_q485758",  
   "merchant": "Starbucks Store, 123",  
   "amount": 402.9,  
   "ts": "2026-01-12 14:16:08 \+0530"  
 },  
 {  
   "id": "b\_a920927",  
   "merchant": "NETFLIX.COM",  
   "amount": 430.86,  
   "ts": "2026-01-12 16:34:18 \+0530"  
 },  
 {  
   "id": "b\_a223119",  
   "merchant": "STARBUCKS.123",  
   "amount": 230.26,  
   "ts": "2026-01-12 16:06:45 \+0530"  
 },  
 {  
   "id": "b\_h457369",  
   "merchant": " starbucks 123 ",  
   "amount": 311.46,  
   "ts": "2026-01-12 15:05:41 \+0530"  
 },  
 {  
   "id": "b\_a968742",  
   "merchant": "Starbucks Store 123",  
   "amount": 319.37,  
   "ts": "2026-01-12 14:13:41 \+0530"  
 },  
 {  
   "id": "b\_v950501",  
   "merchant": "NETFLIX.COM",  
   "amount": 74.49,  
   "ts": "2026-01-12 10:21:44 \+0530"  
 },  
 {  
   "id": "b\_h955730",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 444.22,  
   "ts": "2026-01-12 11:14:17 \+0530"  
 },  
 {  
   "id": "b\_near\_z474689",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 444.13,  
   "ts": "2026-01-12 11:14:29 \+0530"  
 },  
 {  
   "id": "b\_v141474",  
   "merchant": "Local-Grocery",  
   "amount": 417.78,  
   "ts": "2026-01-12 15:36:46 \+0530"  
 },  
 {  
   "id": "b\_l590258",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 112.3,  
   "ts": "2026-01-12 14:02:35 \+0530"  
 },  
 {  
   "id": "b\_s600104",  
   "merchant": "Starbucks Store, 123",  
   "amount": 274.34,  
   "ts": "2026-01-12 15:53:07 \+0530"  
 },  
 {  
   "id": "b\_o900853",  
   "merchant": " starbucks 123 ",  
   "amount": 484.35,  
   "ts": "2026-01-12 17:43:56 \+0530"  
 },  
 {  
   "id": "b\_e743065",  
   "merchant": "Starbucks Store 123",  
   "amount": 394.6,  
   "ts": "2026-01-12 11:51:35 \+0530"  
 },  
 {  
   "id": "b\_q917468",  
   "merchant": "NETFLIX,COM",  
   "amount": 295.45,  
   "ts": "2026-01-12 17:24:02 \+0530"  
 },  
 {  
   "id": "b\_w141825",  
   "merchant": "NETFLIX,COM",  
   "amount": 6.18,  
   "ts": "2026-01-12 17:49:58 \+0530"  
 },  
 {  
   "id": "b\_i901838",  
   "merchant": "UBER BV",  
   "amount": 59.33,  
   "ts": "2026-01-12 17:53:47 \+0530"  
 },  
 {  
   "id": "b\_w385705",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 296.2,  
   "ts": "2026-01-12 10:07:51 \+0530"  
 },  
 {  
   "id": "b\_e753987",  
   "merchant": "amzn mktplace",  
   "amount": 213.18,  
   "ts": "2026-01-12 11:01:58 \+0530"  
 },  
 {  
   "id": "b\_m512262",  
   "merchant": "Uber  Trip",  
   "amount": 258.86,  
   "ts": "2026-01-12 10:59:08 \+0530"  
 },  
 {  
   "id": "b\_o427628",  
   "merchant": "STARBUCKS.123",  
   "amount": 26.18,  
   "ts": "2026-01-12 17:08:17 \+0530"  
 },  
 {  
   "id": "b\_a185802",  
   "merchant": " starbucks 123 ",  
   "amount": 35.06,  
   "ts": "2026-01-12 14:21:05 \+0530"  
 },  
 {  
   "id": "b\_y872642",  
   "merchant": "netflix com",  
   "amount": 357.53,  
   "ts": "2026-01-12 14:02:42 \+0530"  
 },  
 {  
   "id": "b\_y945098",  
   "merchant": "STARBUCKS 123",  
   "amount": 37.24,  
   "ts": "2026-01-12 12:31:18 \+0530"  
 },  
 {  
   "id": "b\_q160843",  
   "merchant": "Uber  Trip",  
   "amount": 193.25,  
   "ts": "2026-01-12 15:27:09 \+0530"  
 },  
 {  
   "id": "b\_v412082",  
   "merchant": "UBER, BV",  
   "amount": 217.78,  
   "ts": "2026-01-12 17:12:42 \+0530"  
 },  
 {  
   "id": "b\_s846951",  
   "merchant": "local grocery",  
   "amount": 361.66,  
   "ts": "2026-01-12 11:57:58 \+0530"  
 },  
 {  
   "id": "b\_x776718",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 172.51,  
   "ts": "2026-01-12 14:07:00 \+0530"  
 },  
 {  
   "id": "b\_n747442",  
   "merchant": "NETFLIX.COM",  
   "amount": 216.27,  
   "ts": "2026-01-12 16:03:59 \+0530"  
 },  
 {  
   "id": "b\_d513810",  
   "merchant": "Fuel Station 9",  
   "amount": 38.22,  
   "ts": "2026-01-12 17:34:26 \+0530"  
 },  
 {  
   "id": "b\_i673687",  
   "merchant": "Starbucks Store, 123",  
   "amount": 406.81,  
   "ts": "2026-01-12 15:03:22 \+0530"  
 },  
 {  
   "id": "b\_c265610",  
   "merchant": "Restaurant ABC",  
   "amount": 168.17,  
   "ts": "2026-01-12 11:07:08 \+0530"  
 },  
 {  
   "id": "b\_near\_w874841",  
   "merchant": "Restaurant ABC",  
   "amount": 168.09,  
   "ts": "2026-01-12 11:07:32 \+0530"  
 },  
 {  
   "id": "b\_x132841",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 434.83,  
   "ts": "2026-01-12 16:34:04 \+0530"  
 },  
 {  
   "id": "b\_i733000",  
   "merchant": "Uber  Trip",  
   "amount": 56.28,  
   "ts": "2026-01-12 14:36:01 \+0530"  
 },  
 {  
   "id": "b\_g446027",  
   "merchant": " netflix com ",  
   "amount": 424.62,  
   "ts": "2026-01-12 13:11:12 \+0530"  
 },  
 {  
   "id": "b\_m512507",  
   "merchant": "UBER, BV",  
   "amount": 308.54,  
   "ts": "2026-01-12 15:51:46 \+0530"  
 },  
 {  
   "id": "b\_s362654",  
   "merchant": "Uber  Trip",  
   "amount": 19.15,  
   "ts": "2026-01-12 12:28:05 \+0530"  
 },  
 {  
   "id": "b\_p504564",  
   "merchant": "Starbucks Store, 123",  
   "amount": 178.67,  
   "ts": "2026-01-12 11:39:06 \+0530"  
 },  
 {  
   "id": "b\_i764894",  
   "merchant": "NETFLIX,COM",  
   "amount": 75.33,  
   "ts": "2026-01-12 15:46:14 \+0530"  
 },  
 {  
   "id": "b\_c530203",  
   "merchant": "Netflix.com",  
   "amount": 191.14,  
   "ts": "2026-01-12 14:07:46 \+0530"  
 },  
 {  
   "id": "b\_x107806",  
   "merchant": "Starbucks Store, 123",  
   "amount": 389.77,  
   "ts": "2026-01-12 15:08:46 \+0530"  
 },  
 {  
   "id": "b\_k355908",  
   "merchant": "Local-Grocery",  
   "amount": 82.64,  
   "ts": "2026-01-12 15:36:51 \+0530"  
 },  
 {  
   "id": "b\_x537385",  
   "merchant": "Amazon   Marketplace",  
   "amount": 332.7,  
   "ts": "2026-01-12 14:23:48 \+0530"  
 },  
 {  
   "id": "b\_a488258",  
   "merchant": "Starbucks Store 123",  
   "amount": 378.8,  
   "ts": "2026-01-12 11:02:42 \+0530"  
 },  
 {  
   "id": "b\_j649374",  
   "merchant": "amzn mktplace",  
   "amount": 340.61,  
   "ts": "2026-01-12 14:24:16 \+0530"  
 },  
 {  
   "id": "b\_d894392",  
   "merchant": "STARBUCKS 123",  
   "amount": 200.88,  
   "ts": "2026-01-12 15:26:05 \+0530"  
 },  
 {  
   "id": "b\_z668592",  
   "merchant": "amzn mktplace",  
   "amount": 384.68,  
   "ts": "2026-01-12 16:58:24 \+0530"  
 },  
 {  
   "id": "b\_j634930",  
   "merchant": "STARBUCKS 123",  
   "amount": 251.29,  
   "ts": "2026-01-12 13:55:40 \+0530"  
 },  
 {  
   "id": "b\_p288994",  
   "merchant": "STARBUCKS.123",  
   "amount": 252.77,  
   "ts": "2026-01-12 17:35:54 \+0530"  
 },  
 {  
   "id": "b\_w727523",  
   "merchant": "starbucks 123",  
   "amount": 431.55,  
   "ts": "2026-01-12 10:21:58 \+0530"  
 },  
 {  
   "id": "b\_q289708",  
   "merchant": "Uber Trip",  
   "amount": 431.65,  
   "ts": "2026-01-12 15:14:58 \+0530"  
 },  
 {  
   "id": "b\_o457984",  
   "merchant": "starbucks 123",  
   "amount": 148.48,  
   "ts": "2026-01-12 13:42:09 \+0530"  
 },  
 {  
   "id": "b\_near\_q628520",  
   "merchant": "starbucks 123",  
   "amount": 148.56,  
   "ts": "2026-01-12 13:42:21 \+0530"  
 },  
 {  
   "id": "b\_c821807",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 232.66,  
   "ts": "2026-01-12 16:18:05 \+0530"  
 },  
 {  
   "id": "b\_o562138",  
   "merchant": "Starbucks Store 123",  
   "amount": 125.67,  
   "ts": "2026-01-12 15:21:32 \+0530"  
 },  
 {  
   "id": "b\_t757641",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 374.16,  
   "ts": "2026-01-12 12:00:54 \+0530"  
 },  
 {  
   "id": "b\_z651114",  
   "merchant": "Amazon   Marketplace",  
   "amount": 318.32,  
   "ts": "2026-01-12 14:48:46 \+0530"  
 },  
 {  
   "id": "b\_p125186",  
   "merchant": "Local-Grocery",  
   "amount": 178.68,  
   "ts": "2026-01-12 17:27:18 \+0530"  
 },  
 {  
   "id": "b\_q785595",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 77.34,  
   "ts": "2026-01-12 11:47:20 \+0530"  
 },  
 {  
   "id": "b\_y404216",  
   "merchant": "amzn mktplace",  
   "amount": 42.49,  
   "ts": "2026-01-12 17:03:25 \+0530"  
 },  
 {  
   "id": "b\_n254910",  
   "merchant": "UBER, BV",  
   "amount": 381.23,  
   "ts": "2026-01-12 14:06:39 \+0530"  
 },  
 {  
   "id": "b\_x185080",  
   "merchant": "NETFLIX,COM",  
   "amount": 165.11,  
   "ts": "2026-01-12 15:43:09 \+0530"  
 },  
 {  
   "id": "b\_x838248",  
   "merchant": "Uber  Trip",  
   "amount": 256.0,  
   "ts": "2026-01-12 12:48:13 \+0530"  
 },  
 {  
   "id": "b\_j563059",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 416.48,  
   "ts": "2026-01-12 10:42:53 \+0530"  
 },  
 {  
   "id": "b\_z809027",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 256.2,  
   "ts": "2026-01-12 14:12:23 \+0530"  
 },  
 {  
   "id": "b\_u828275",  
   "merchant": "Coffee Shop 77",  
   "amount": 227.4,  
   "ts": "2026-01-12 17:22:05 \+0530"  
 },  
 {  
   "id": "b\_w818542",  
   "merchant": "Amazon   Marketplace",  
   "amount": 240.45,  
   "ts": "2026-01-12 11:22:40 \+0530"  
 },  
 {  
   "id": "b\_w662279",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 491.44,  
   "ts": "2026-01-12 10:46:07 \+0530"  
 },  
 {  
   "id": "b\_y703640",  
   "merchant": "Amazon   Marketplace",  
   "amount": 355.19,  
   "ts": "2026-01-12 13:58:49 \+0530"  
 },  
 {  
   "id": "b\_t280656",  
   "merchant": "Uber  Trip",  
   "amount": 189.41,  
   "ts": "2026-01-12 12:33:28 \+0530"  
 },  
 {  
   "id": "b\_u311245",  
   "merchant": "NETFLIX,COM",  
   "amount": 30.62,  
   "ts": "2026-01-12 15:43:04 \+0530"  
 },  
 {  
   "id": "b\_k273125",  
   "merchant": "amzn mktplace",  
   "amount": 281.02,  
   "ts": "2026-01-12 13:31:29 \+0530"  
 },  
 {  
   "id": "b\_g999448",  
   "merchant": "NETFLIX,COM",  
   "amount": 437.31,  
   "ts": "2026-01-12 13:02:26 \+0530"  
 },  
 {  
   "id": "b\_s972545",  
   "merchant": "NETFLIX.COM",  
   "amount": 158.39,  
   "ts": "2026-01-12 15:59:34 \+0530"  
 },  
 {  
   "id": "b\_d279305",  
   "merchant": "STARBUCKS.123",  
   "amount": 355.79,  
   "ts": "2026-01-12 10:46:43 \+0530"  
 },  
 {  
   "id": "b\_v742307",  
   "merchant": "Starbucks Store, 123",  
   "amount": 407.3,  
   "ts": "2026-01-12 16:05:11 \+0530"  
 },  
 {  
   "id": "b\_a800512",  
   "merchant": "Restaurant ABC",  
   "amount": 51.11,  
   "ts": "2026-01-12 16:05:50 \+0530"  
 },  
 {  
   "id": "b\_a343709",  
   "merchant": "Starbucks Store, 123",  
   "amount": 157.77,  
   "ts": "2026-01-12 11:57:03 \+0530"  
 },  
 {  
   "id": "b\_r361882",  
   "merchant": "Uber Trip",  
   "amount": 499.83,  
   "ts": "2026-01-12 11:23:58 \+0530"  
 },  
 {  
   "id": "b\_t972413",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 256.07,  
   "ts": "2026-01-12 14:54:01 \+0530"  
 },  
 {  
   "id": "b\_q972471",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 439.03,  
   "ts": "2026-01-12 15:37:52 \+0530"  
 },  
 {  
   "id": "b\_x851241",  
   "merchant": "STARBUCKS.123",  
   "amount": 94.34,  
   "ts": "2026-01-12 16:19:26 \+0530"  
 },  
 {  
   "id": "b\_q705060",  
   "merchant": "Local-Grocery",  
   "amount": 361.98,  
   "ts": "2026-01-12 10:25:09 \+0530"  
 },  
 {  
   "id": "b\_e580501",  
   "merchant": "NETFLIX.COM",  
   "amount": 485.1,  
   "ts": "2026-01-12 15:33:14 \+0530"  
 },  
 {  
   "id": "b\_v176143",  
   "merchant": "netflix com",  
   "amount": 121.03,  
   "ts": "2026-01-12 12:11:43 \+0530"  
 },  
 {  
   "id": "b\_t916305",  
   "merchant": "LOCAL GROCERY",  
   "amount": 187.06,  
   "ts": "2026-01-12 15:11:30 \+0530"  
 },  
 {  
   "id": "b\_a550263",  
   "merchant": "netflix com",  
   "amount": 253.86,  
   "ts": "2026-01-12 13:48:04 \+0530"  
 },  
 {  
   "id": "b\_u696766",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 73.64,  
   "ts": "2026-01-12 12:15:22 \+0530"  
 },  
 {  
   "id": "b\_n852982",  
   "merchant": "Uber Trip",  
   "amount": 146.3,  
   "ts": "2026-01-12 16:55:32 \+0530"  
 },  
 {  
   "id": "b\_k430414",  
   "merchant": "UBER, BV",  
   "amount": 78.09,  
   "ts": "2026-01-12 13:58:52 \+0530"  
 },  
 {  
   "id": "b\_t153562",  
   "merchant": "Local-Grocery",  
   "amount": 465.36,  
   "ts": "2026-01-12 13:45:11 \+0530"  
 },  
 {  
   "id": "b\_l144189",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 420.44,  
   "ts": "2026-01-12 15:54:05 \+0530"  
 },  
 {  
   "id": "b\_d695881",  
   "merchant": "Amazon Marketplace",  
   "amount": 147.07,  
   "ts": "2026-01-12 17:32:09 \+0530"  
 },  
 {  
   "id": "b\_a359591",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 226.97,  
   "ts": "2026-01-12 10:03:04 \+0530"  
 },  
 {  
   "id": "b\_o882339",  
   "merchant": "STARBUCKS.123",  
   "amount": 276.51,  
   "ts": "2026-01-12 16:37:34 \+0530"  
 },  
 {  
   "id": "b\_a367594",  
   "merchant": "Starbucks Store, 123",  
   "amount": 122.64,  
   "ts": "2026-01-12 11:07:21 \+0530"  
 },  
 {  
   "id": "b\_m761146",  
   "merchant": "STARBUCKS.123",  
   "amount": 231.3,  
   "ts": "2026-01-12 11:00:17 \+0530"  
 },  
 {  
   "id": "b\_q770842",  
   "merchant": "UBER, BV",  
   "amount": 289.84,  
   "ts": "2026-01-12 16:31:40 \+0530"  
 },  
 {  
   "id": "b\_g234695",  
   "merchant": "STARBUCKS.123",  
   "amount": 179.59,  
   "ts": "2026-01-12 15:10:58 \+0530"  
 },  
 {  
   "id": "b\_v305874",  
   "merchant": "NETFLIX.COM",  
   "amount": 264.35,  
   "ts": "2026-01-12 13:16:21 \+0530"  
 },  
 {  
   "id": "b\_t680215",  
   "merchant": "UBER, BV",  
   "amount": 279.25,  
   "ts": "2026-01-12 13:33:45 \+0530"  
 },  
 {  
   "id": "b\_w789009",  
   "merchant": "NETFLIX.COM",  
   "amount": 130.71,  
   "ts": "2026-01-12 10:09:19 \+0530"  
 },  
 {  
   "id": "b\_t543646",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 367.64,  
   "ts": "2026-01-12 15:42:11 \+0530"  
 },  
 {  
   "id": "b\_h942433",  
   "merchant": "Uber  Trip",  
   "amount": 70.91,  
   "ts": "2026-01-12 13:11:26 \+0530"  
 },  
 {  
   "id": "b\_f683506",  
   "merchant": "Local Grocery",  
   "amount": 416.15,  
   "ts": "2026-01-12 16:12:32 \+0530"  
 },  
 {  
   "id": "b\_i660705",  
   "merchant": "Uber  Trip",  
   "amount": 406.76,  
   "ts": "2026-01-12 10:34:28 \+0530"  
 },  
 {  
   "id": "b\_y491895",  
   "merchant": "Coffee Shop 77",  
   "amount": 150.53,  
   "ts": "2026-01-12 10:23:51 \+0530"  
 },  
 {  
   "id": "b\_u419229",  
   "merchant": "Local-Grocery",  
   "amount": 467.14,  
   "ts": "2026-01-12 14:40:31 \+0530"  
 },  
 {  
   "id": "b\_k751225",  
   "merchant": "Uber  Trip",  
   "amount": 23.27,  
   "ts": "2026-01-12 15:53:45 \+0530"  
 },  
 {  
   "id": "b\_p389702",  
   "merchant": "Starbucks Store, 123",  
   "amount": 433.68,  
   "ts": "2026-01-12 13:00:56 \+0530"  
 },  
 {  
   "id": "b\_e596423",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 445.27,  
   "ts": "2026-01-12 16:46:01 \+0530"  
 },  
 {  
   "id": "b\_b801913",  
   "merchant": "UBER, BV",  
   "amount": 364.89,  
   "ts": "2026-01-12 10:44:12 \+0530"  
 },  
 {  
   "id": "b\_s887619",  
   "merchant": "Ecomm Seller X",  
   "amount": 274.96,  
   "ts": "2026-01-12 12:54:43 \+0530"  
 },  
 {  
   "id": "b\_p570436",  
   "merchant": "Starbucks Store, 123",  
   "amount": 156.74,  
   "ts": "2026-01-12 15:29:36 \+0530"  
 },  
 {  
   "id": "b\_c900881",  
   "merchant": "uber trip",  
   "amount": 140.78,  
   "ts": "2026-01-12 17:13:30 \+0530"  
 },  
 {  
   "id": "b\_o374759",  
   "merchant": "amzn mktplace",  
   "amount": 75.96,  
   "ts": "2026-01-12 15:31:58 \+0530"  
 },  
 {  
   "id": "b\_m428607",  
   "merchant": "Starbucks Store, 123",  
   "amount": 38.3,  
   "ts": "2026-01-12 17:05:51 \+0530"  
 },  
 {  
   "id": "b\_d550348",  
   "merchant": "STARBUCKS.123",  
   "amount": 21.33,  
   "ts": "2026-01-12 11:22:29 \+0530"  
 },  
 {  
   "id": "b\_s620284",  
   "merchant": "uber trip",  
   "amount": 160.91,  
   "ts": "2026-01-12 12:32:24 \+0530"  
 },  
 {  
   "id": "b\_d664796",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 251.75,  
   "ts": "2026-01-12 13:31:59 \+0530"  
 },  
 {  
   "id": "b\_v964549",  
   "merchant": "Starbucks Store, 123",  
   "amount": 353.76,  
   "ts": "2026-01-12 11:58:02 \+0530"  
 },  
 {  
   "id": "b\_k532992",  
   "merchant": "netflix com",  
   "amount": 103.86,  
   "ts": "2026-01-12 11:15:10 \+0530"  
 },  
 {  
   "id": "b\_e186377",  
   "merchant": "amzn mktplace",  
   "amount": 225.1,  
   "ts": "2026-01-12 10:48:56 \+0530"  
 },  
 {  
   "id": "b\_p827805",  
   "merchant": "Pharmacy 22",  
   "amount": 335.39,  
   "ts": "2026-01-12 16:26:32 \+0530"  
 },  
 {  
   "id": "b\_near\_s161101",  
   "merchant": "Pharmacy 22",  
   "amount": 335.33,  
   "ts": "2026-01-12 16:26:28 \+0530"  
 },  
 {  
   "id": "b\_o817052",  
   "merchant": "Uber  Trip",  
   "amount": 426.42,  
   "ts": "2026-01-12 16:27:49 \+0530"  
 },  
 {  
   "id": "b\_t399130",  
   "merchant": "NETFLIX.COM",  
   "amount": 256.87,  
   "ts": "2026-01-12 11:04:49 \+0530"  
 },  
 {  
   "id": "b\_near\_i889834",  
   "merchant": "NETFLIX.COM",  
   "amount": 256.9,  
   "ts": "2026-01-12 11:05:11 \+0530"  
 },  
 {  
   "id": "b\_f293961",  
   "merchant": "Amazon   Marketplace",  
   "amount": 17.75,  
   "ts": "2026-01-12 15:01:33 \+0530"  
 },  
 {  
   "id": "b\_c658605",  
   "merchant": "UBER, BV",  
   "amount": 69.49,  
   "ts": "2026-01-12 10:46:00 \+0530"  
 },  
 {  
   "id": "b\_d904991",  
   "merchant": "starbucks 123",  
   "amount": 265.14,  
   "ts": "2026-01-12 16:58:35 \+0530"  
 },  
 {  
   "id": "b\_l859517",  
   "merchant": "Uber  Trip",  
   "amount": 486.04,  
   "ts": "2026-01-12 15:51:45 \+0530"  
 },  
 {  
   "id": "b\_d626732",  
   "merchant": "UBER, BV",  
   "amount": 175.81,  
   "ts": "2026-01-12 18:00:28 \+0530"  
 },  
 {  
   "id": "b\_g682091",  
   "merchant": "NETFLIX.COM",  
   "amount": 385.24,  
   "ts": "2026-01-12 11:08:58 \+0530"  
 },  
 {  
   "id": "b\_f267747",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 416.64,  
   "ts": "2026-01-12 14:41:28 \+0530"  
 },  
 {  
   "id": "b\_a319347",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 467.86,  
   "ts": "2026-01-12 12:03:36 \+0530"  
 },  
 {  
   "id": "b\_h531856",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 308.91,  
   "ts": "2026-01-12 10:52:02 \+0530"  
 },  
 {  
   "id": "b\_z956859",  
   "merchant": "Local-Grocery",  
   "amount": 422.32,  
   "ts": "2026-01-12 15:08:55 \+0530"  
 },  
 {  
   "id": "b\_i324048",  
   "merchant": "LOCAL GROCERY",  
   "amount": 27.28,  
   "ts": "2026-01-12 15:49:37 \+0530"  
 },  
 {  
   "id": "b\_y645918",  
   "merchant": "Starbucks Store 123",  
   "amount": 384.35,  
   "ts": "2026-01-12 13:49:00 \+0530"  
 },  
 {  
   "id": "b\_t573176",  
   "merchant": "amzn mktplace",  
   "amount": 275.12,  
   "ts": "2026-01-12 15:49:47 \+0530"  
 },  
 {  
   "id": "b\_r491493",  
   "merchant": "starbucks 123",  
   "amount": 152.36,  
   "ts": "2026-01-12 10:38:23 \+0530"  
 },  
 {  
   "id": "b\_l141889",  
   "merchant": "Amazon   Marketplace",  
   "amount": 244.64,  
   "ts": "2026-01-12 11:50:25 \+0530"  
 },  
 {  
   "id": "b\_e813184",  
   "merchant": "amzn mktplace",  
   "amount": 162.9,  
   "ts": "2026-01-12 17:49:06 \+0530"  
 },  
 {  
   "id": "b\_c128757",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 259.69,  
   "ts": "2026-01-12 10:59:26 \+0530"  
 },  
 {  
   "id": "b\_y692240",  
   "merchant": "Uber  Trip",  
   "amount": 227.69,  
   "ts": "2026-01-12 11:18:03 \+0530"  
 },  
 {  
   "id": "b\_e142707",  
   "merchant": "starbucks 123",  
   "amount": 432.77,  
   "ts": "2026-01-12 13:49:20 \+0530"  
 },  
 {  
   "id": "b\_a751623",  
   "merchant": "local grocery",  
   "amount": 362.19,  
   "ts": "2026-01-12 11:44:12 \+0530"  
 },  
 {  
   "id": "b\_a240162",  
   "merchant": "Amazon   Marketplace",  
   "amount": 254.94,  
   "ts": "2026-01-12 12:10:49 \+0530"  
 },  
 {  
   "id": "b\_x271940",  
   "merchant": "UBER, BV",  
   "amount": 8.86,  
   "ts": "2026-01-12 10:57:36 \+0530"  
 },  
 {  
   "id": "b\_t195941",  
   "merchant": "Local-Grocery",  
   "amount": 383.4,  
   "ts": "2026-01-12 13:21:11 \+0530"  
 },  
 {  
   "id": "b\_m268799",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 101.19,  
   "ts": "2026-01-12 15:01:31 \+0530"  
 },  
 {  
   "id": "b\_f831193",  
   "merchant": "starbucks 123",  
   "amount": 383.22,  
   "ts": "2026-01-12 12:31:19 \+0530"  
 },  
 {  
   "id": "b\_t896487",  
   "merchant": "UBER, BV",  
   "amount": 487.66,  
   "ts": "2026-01-12 13:34:04 \+0530"  
 },  
 {  
   "id": "b\_b734406",  
   "merchant": "Restaurant ABC",  
   "amount": 469.96,  
   "ts": "2026-01-12 16:52:01 \+0530"  
 },  
 {  
   "id": "b\_u335129",  
   "merchant": "uber trip",  
   "amount": 252.68,  
   "ts": "2026-01-12 10:25:39 \+0530"  
 },  
 {  
   "id": "b\_r678874",  
   "merchant": "netflix com",  
   "amount": 262.28,  
   "ts": "2026-01-12 11:22:40 \+0530"  
 },  
 {  
   "id": "b\_x336405",  
   "merchant": "Amazon   Marketplace",  
   "amount": 162.6,  
   "ts": "2026-01-12 13:35:15 \+0530"  
 },  
 {  
   "id": "b\_r420860",  
   "merchant": "local grocery",  
   "amount": 171.7,  
   "ts": "2026-01-12 17:32:14 \+0530"  
 },  
 {  
   "id": "b\_h411359",  
   "merchant": "STARBUCKS.123",  
   "amount": 489.9,  
   "ts": "2026-01-12 15:19:25 \+0530"  
 },  
 {  
   "id": "b\_m497640",  
   "merchant": "Local-Grocery",  
   "amount": 300.73,  
   "ts": "2026-01-12 12:25:11 \+0530"  
 },  
 {  
   "id": "b\_n996933",  
   "merchant": "Restaurant ABC",  
   "amount": 290.96,  
   "ts": "2026-01-12 16:33:29 \+0530"  
 },  
 {  
   "id": "b\_m178591",  
   "merchant": "UBER, BV",  
   "amount": 316.32,  
   "ts": "2026-01-12 15:39:50 \+0530"  
 },  
 {  
   "id": "b\_y865651",  
   "merchant": "Starbucks Store, 123",  
   "amount": 20.53,  
   "ts": "2026-01-12 16:21:07 \+0530"  
 },  
 {  
   "id": "b\_b150863",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 212.14,  
   "ts": "2026-01-12 13:46:51 \+0530"  
 },  
 {  
   "id": "b\_w854480",  
   "merchant": "STARBUCKS.123",  
   "amount": 460.88,  
   "ts": "2026-01-12 17:56:08 \+0530"  
 },  
 {  
   "id": "b\_near\_u861338",  
   "merchant": "STARBUCKS.123",  
   "amount": 460.94,  
   "ts": "2026-01-12 17:55:44 \+0530"  
 },  
 {  
   "id": "b\_j333315",  
   "merchant": "Amazon   Marketplace",  
   "amount": 261.95,  
   "ts": "2026-01-12 15:07:18 \+0530"  
 },  
 {  
   "id": "b\_m274470",  
   "merchant": "uber trip",  
   "amount": 160.56,  
   "ts": "2026-01-12 11:35:37 \+0530"  
 },  
 {  
   "id": "b\_d164103",  
   "merchant": "Starbucks Store, 123",  
   "amount": 339.84,  
   "ts": "2026-01-12 17:54:43 \+0530"  
 },  
 {  
   "id": "b\_z108808",  
   "merchant": "NETFLIX.COM",  
   "amount": 307.15,  
   "ts": "2026-01-12 11:46:22 \+0530"  
 },  
 {  
   "id": "b\_k101875",  
   "merchant": "uber trip",  
   "amount": 471.3,  
   "ts": "2026-01-12 12:41:17 \+0530"  
 },  
 {  
   "id": "b\_c531128",  
   "merchant": "NETFLIX.COM",  
   "amount": 65.53,  
   "ts": "2026-01-12 16:09:33 \+0530"  
 },  
 {  
   "id": "b\_q277387",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 409.91,  
   "ts": "2026-01-12 17:02:42 \+0530"  
 },  
 {  
   "id": "b\_c124898",  
   "merchant": "Starbucks Store, 123",  
   "amount": 477.89,  
   "ts": "2026-01-12 10:42:24 \+0530"  
 },  
 {  
   "id": "b\_x898355",  
   "merchant": "netflix com",  
   "amount": 110.64,  
   "ts": "2026-01-12 17:31:41 \+0530"  
 },  
 {  
   "id": "b\_x259270",  
   "merchant": "Pharmacy 22",  
   "amount": 73.13,  
   "ts": "2026-01-12 15:35:07 \+0530"  
 },  
 {  
   "id": "b\_w652434",  
   "merchant": "STARBUCKS.123",  
   "amount": 209.46,  
   "ts": "2026-01-12 16:31:40 \+0530"  
 },  
 {  
   "id": "b\_q584438",  
   "merchant": "NETFLIX.COM",  
   "amount": 7.79,  
   "ts": "2026-01-12 15:01:30 \+0530"  
 },  
 {  
   "id": "b\_j299803",  
   "merchant": "netflix com",  
   "amount": 241.19,  
   "ts": "2026-01-12 11:47:40 \+0530"  
 },  
 {  
   "id": "b\_k705032",  
   "merchant": "STARBUCKS.123",  
   "amount": 454.19,  
   "ts": "2026-01-12 10:50:12 \+0530"  
 },  
 {  
   "id": "b\_c687064",  
   "merchant": "STARBUCKS.123",  
   "amount": 457.07,  
   "ts": "2026-01-12 11:34:16 \+0530"  
 },  
 {  
   "id": "b\_l441980",  
   "merchant": "amzn mktplace",  
   "amount": 481.01,  
   "ts": "2026-01-12 12:37:36 \+0530"  
 },  
 {  
   "id": "b\_v821825",  
   "merchant": "Restaurant ABC",  
   "amount": 264.74,  
   "ts": "2026-01-12 11:12:18 \+0530"  
 },  
 {  
   "id": "b\_k188154",  
   "merchant": "STARBUCKS.123",  
   "amount": 12.23,  
   "ts": "2026-01-12 13:04:03 \+0530"  
 },  
 {  
   "id": "b\_h941034",  
   "merchant": "local grocery",  
   "amount": 167.23,  
   "ts": "2026-01-12 13:39:30 \+0530"  
 },  
 {  
   "id": "b\_p164171",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 241.15,  
   "ts": "2026-01-12 12:14:15 \+0530"  
 },  
 {  
   "id": "b\_u373887",  
   "merchant": "Netflix.com",  
   "amount": 134.24,  
   "ts": "2026-01-12 13:14:55 \+0530"  
 },  
 {  
   "id": "b\_v295396",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 235.21,  
   "ts": "2026-01-12 11:28:25 \+0530"  
 },  
 {  
   "id": "b\_l622367",  
   "merchant": "Uber  Trip",  
   "amount": 346.67,  
   "ts": "2026-01-12 15:48:55 \+0530"  
 },  
 {  
   "id": "b\_near\_r984878",  
   "merchant": "Uber  Trip",  
   "amount": 346.74,  
   "ts": "2026-01-12 15:49:09 \+0530"  
 },  
 {  
   "id": "b\_g133421",  
   "merchant": "UBER BV",  
   "amount": 478.77,  
   "ts": "2026-01-12 14:22:37 \+0530"  
 },  
 {  
   "id": "b\_s930269",  
   "merchant": "NETFLIX,COM",  
   "amount": 190.2,  
   "ts": "2026-01-12 16:23:07 \+0530"  
 },  
 {  
   "id": "b\_i948731",  
   "merchant": "Netflix.com",  
   "amount": 381.89,  
   "ts": "2026-01-12 16:10:52 \+0530"  
 },  
 {  
   "id": "b\_o846840",  
   "merchant": "local grocery",  
   "amount": 265.11,  
   "ts": "2026-01-12 14:49:12 \+0530"  
 },  
 {  
   "id": "b\_t391511",  
   "merchant": "Amazon   Marketplace",  
   "amount": 60.25,  
   "ts": "2026-01-12 17:32:48 \+0530"  
 },  
 {  
   "id": "b\_m219867",  
   "merchant": "amzn mktplace",  
   "amount": 79.02,  
   "ts": "2026-01-12 13:00:10 \+0530"  
 },  
 {  
   "id": "b\_u457183",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 359.76,  
   "ts": "2026-01-12 14:26:38 \+0530"  
 },  
 {  
   "id": "b\_y290611",  
   "merchant": "UBER, BV",  
   "amount": 233.62,  
   "ts": "2026-01-12 17:21:23 \+0530"  
 },  
 {  
   "id": "b\_h947881",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 409.33,  
   "ts": "2026-01-12 16:47:05 \+0530"  
 },  
 {  
   "id": "b\_u228719",  
   "merchant": "Amazon   Marketplace",  
   "amount": 175.26,  
   "ts": "2026-01-12 10:34:53 \+0530"  
 },  
 {  
   "id": "b\_m236405",  
   "merchant": "Coffee Shop 77",  
   "amount": 493.16,  
   "ts": "2026-01-12 14:18:30 \+0530"  
 },  
 {  
   "id": "b\_t715277",  
   "merchant": "Starbucks Store 123",  
   "amount": 384.26,  
   "ts": "2026-01-12 16:39:47 \+0530"  
 },  
 {  
   "id": "b\_n452629",  
   "merchant": "starbucks 123",  
   "amount": 227.09,  
   "ts": "2026-01-12 13:26:10 \+0530"  
 },  
 {  
   "id": "b\_near\_k997479",  
   "merchant": "starbucks 123",  
   "amount": 227.18,  
   "ts": "2026-01-12 13:25:45 \+0530"  
 },  
 {  
   "id": "b\_w569086",  
   "merchant": "NETFLIX,COM",  
   "amount": 71.08,  
   "ts": "2026-01-12 17:49:08 \+0530"  
 },  
 {  
   "id": "b\_l797877",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 199.48,  
   "ts": "2026-01-12 11:10:23 \+0530"  
 },  
 {  
   "id": "b\_t161331",  
   "merchant": "LOCAL GROCERY",  
   "amount": 489.55,  
   "ts": "2026-01-12 14:14:02 \+0530"  
 },  
 {  
   "id": "b\_o232849",  
   "merchant": "amzn mktplace",  
   "amount": 212.31,  
   "ts": "2026-01-12 16:57:49 \+0530"  
 },  
 {  
   "id": "b\_j526434",  
   "merchant": "Amazon Marketplace",  
   "amount": 299.12,  
   "ts": "2026-01-12 14:03:22 \+0530"  
 },  
 {  
   "id": "b\_near\_s786711",  
   "merchant": "Amazon Marketplace",  
   "amount": 299.17,  
   "ts": "2026-01-12 14:03:25 \+0530"  
 },  
 {  
   "id": "b\_z641473",  
   "merchant": "NETFLIX",  
   "amount": 125.98,  
   "ts": "2026-01-12 10:25:30 \+0530"  
 },  
 {  
   "id": "b\_p494364",  
   "merchant": "STARBUCKS.123",  
   "amount": 378.37,  
   "ts": "2026-01-12 17:02:28 \+0530"  
 },  
 {  
   "id": "b\_y480034",  
   "merchant": "Uber  Trip",  
   "amount": 120.94,  
   "ts": "2026-01-12 16:40:44 \+0530"  
 },  
 {  
   "id": "b\_c296418",  
   "merchant": "Uber  Trip",  
   "amount": 122.46,  
   "ts": "2026-01-12 17:57:14 \+0530"  
 },  
 {  
   "id": "b\_w915629",  
   "merchant": "uber trip",  
   "amount": 153.54,  
   "ts": "2026-01-12 10:32:13 \+0530"  
 },  
 {  
   "id": "b\_l148314",  
   "merchant": "Local-Grocery",  
   "amount": 314.65,  
   "ts": "2026-01-12 13:54:13 \+0530"  
 },  
 {  
   "id": "b\_c436717",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 471.23,  
   "ts": "2026-01-12 16:05:50 \+0530"  
 },  
 {  
   "id": "b\_f823459",  
   "merchant": "netflix com",  
   "amount": 123.99,  
   "ts": "2026-01-12 12:01:56 \+0530"  
 },  
 {  
   "id": "b\_j127702",  
   "merchant": "netflix com",  
   "amount": 457.62,  
   "ts": "2026-01-12 15:07:52 \+0530"  
 },  
 {  
   "id": "b\_g509056",  
   "merchant": "Uber  Trip",  
   "amount": 197.23,  
   "ts": "2026-01-12 12:19:03 \+0530"  
 },  
 {  
   "id": "b\_x275921",  
   "merchant": "netflix com",  
   "amount": 424.06,  
   "ts": "2026-01-12 10:49:18 \+0530"  
 },  
 {  
   "id": "b\_m348888",  
   "merchant": "local grocery",  
   "amount": 137.97,  
   "ts": "2026-01-12 16:08:35 \+0530"  
 },  
 {  
   "id": "b\_n595354",  
   "merchant": "Amazon   Marketplace",  
   "amount": 97.94,  
   "ts": "2026-01-12 16:55:21 \+0530"  
 },  
 {  
   "id": "b\_p826629",  
   "merchant": "NETFLIX.COM",  
   "amount": 242.22,  
   "ts": "2026-01-12 17:54:08 \+0530"  
 },  
 {  
   "id": "b\_t660991",  
   "merchant": "Coffee Shop 77",  
   "amount": 109.82,  
   "ts": "2026-01-12 11:45:03 \+0530"  
 },  
 {  
   "id": "b\_l640887",  
   "merchant": "netflix com",  
   "amount": 457.61,  
   "ts": "2026-01-12 16:21:07 \+0530"  
 },  
 {  
   "id": "b\_p941225",  
   "merchant": "uber trip",  
   "amount": 382.26,  
   "ts": "2026-01-12 12:54:47 \+0530"  
 },  
 {  
   "id": "b\_m303104",  
   "merchant": "STARBUCKS 123",  
   "amount": 283.64,  
   "ts": "2026-01-12 11:59:28 \+0530"  
 },  
 {  
   "id": "b\_m779870",  
   "merchant": "amzn mktplace",  
   "amount": 467.78,  
   "ts": "2026-01-12 15:15:46 \+0530"  
 },  
 {  
   "id": "b\_y874463",  
   "merchant": " starbucks 123 ",  
   "amount": 168.1,  
   "ts": "2026-01-12 13:27:13 \+0530"  
 },  
 {  
   "id": "b\_m682990",  
   "merchant": "STARBUCKS.123",  
   "amount": 328.94,  
   "ts": "2026-01-12 16:13:14 \+0530"  
 },  
 {  
   "id": "b\_j859813",  
   "merchant": "Uber  Trip",  
   "amount": 88.62,  
   "ts": "2026-01-12 16:32:50 \+0530"  
 },  
 {  
   "id": "b\_f883566",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 472.98,  
   "ts": "2026-01-12 16:06:02 \+0530"  
 },  
 {  
   "id": "b\_r772539",  
   "merchant": "Amazon   Marketplace",  
   "amount": 422.03,  
   "ts": "2026-01-12 12:06:28 \+0530"  
 },  
 {  
   "id": "b\_p541019",  
   "merchant": "NETFLIX,COM",  
   "amount": 456.59,  
   "ts": "2026-01-12 15:59:38 \+0530"  
 },  
 {  
   "id": "b\_y388928",  
   "merchant": "amzn mktplace",  
   "amount": 208.77,  
   "ts": "2026-01-12 10:09:37 \+0530"  
 },  
 {  
   "id": "b\_h865486",  
   "merchant": "Starbucks Store, 123",  
   "amount": 13.21,  
   "ts": "2026-01-12 17:25:28 \+0530"  
 },  
 {  
   "id": "b\_s545362",  
   "merchant": "STARBUCKS.123",  
   "amount": 58.08,  
   "ts": "2026-01-12 10:52:16 \+0530"  
 },  
 {  
   "id": "b\_k794264",  
   "merchant": "STARBUCKS.123",  
   "amount": 265.66,  
   "ts": "2026-01-12 17:26:15 \+0530"  
 },  
 {  
   "id": "b\_near\_q352013",  
   "merchant": "STARBUCKS.123",  
   "amount": 265.64,  
   "ts": "2026-01-12 17:26:06 \+0530"  
 },  
 {  
   "id": "b\_z491240",  
   "merchant": "Local-Grocery",  
   "amount": 189.66,  
   "ts": "2026-01-12 13:41:45 \+0530"  
 },  
 {  
   "id": "b\_v404062",  
   "merchant": "STARBUCKS 123",  
   "amount": 153.22,  
   "ts": "2026-01-12 17:33:19 \+0530"  
 },  
 {  
   "id": "b\_u782331",  
   "merchant": "UBER, BV",  
   "amount": 353.18,  
   "ts": "2026-01-12 10:44:43 \+0530"  
 },  
 {  
   "id": "b\_o715175",  
   "merchant": "Starbucks Store, 123",  
   "amount": 284.95,  
   "ts": "2026-01-12 17:48:22 \+0530"  
 },  
 {  
   "id": "b\_v508094",  
   "merchant": "local grocery",  
   "amount": 109.81,  
   "ts": "2026-01-12 17:53:55 \+0530"  
 },  
 {  
   "id": "b\_l444912",  
   "merchant": "UBER BV",  
   "amount": 462.38,  
   "ts": "2026-01-12 16:13:49 \+0530"  
 },  
 {  
   "id": "b\_s700007",  
   "merchant": "Restaurant ABC",  
   "amount": 76.55,  
   "ts": "2026-01-12 17:28:39 \+0530"  
 },  
 {  
   "id": "b\_r347050",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 339.88,  
   "ts": "2026-01-12 14:55:33 \+0530"  
 },  
 {  
   "id": "b\_q637497",  
   "merchant": "Uber  Trip",  
   "amount": 31.76,  
   "ts": "2026-01-12 13:59:48 \+0530"  
 },  
 {  
   "id": "b\_r395802",  
   "merchant": "Amazon Marketplace",  
   "amount": 385.91,  
   "ts": "2026-01-12 15:23:56 \+0530"  
 },  
 {  
   "id": "b\_h388909",  
   "merchant": "Starbucks Store, 123",  
   "amount": 392.86,  
   "ts": "2026-01-12 14:06:21 \+0530"  
 },  
 {  
   "id": "b\_i377715",  
   "merchant": "starbucks 123",  
   "amount": 219.0,  
   "ts": "2026-01-12 16:12:13 \+0530"  
 },  
 {  
   "id": "b\_g823528",  
   "merchant": "STARBUCKS.123",  
   "amount": 151.14,  
   "ts": "2026-01-12 12:14:17 \+0530"  
 },  
 {  
   "id": "b\_q195923",  
   "merchant": "Amazon Marketplace",  
   "amount": 193.26,  
   "ts": "2026-01-12 17:07:20 \+0530"  
 },  
 {  
   "id": "b\_j896739",  
   "merchant": "Uber  Trip",  
   "amount": 55.37,  
   "ts": "2026-01-12 10:47:21 \+0530"  
 },  
 {  
   "id": "b\_c462268",  
   "merchant": " starbucks 123 ",  
   "amount": 300.19,  
   "ts": "2026-01-12 17:08:10 \+0530"  
 },  
 {  
   "id": "b\_l758250",  
   "merchant": "Amazon   Marketplace",  
   "amount": 197.01,  
   "ts": "2026-01-12 10:46:21 \+0530"  
 },  
 {  
   "id": "b\_d611980",  
   "merchant": "Local-Grocery",  
   "amount": 288.25,  
   "ts": "2026-01-12 11:33:25 \+0530"  
 },  
 {  
   "id": "b\_o388742",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 154.15,  
   "ts": "2026-01-12 14:29:07 \+0530"  
 },  
 {  
   "id": "b\_m514961",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 213.52,  
   "ts": "2026-01-12 14:25:36 \+0530"  
 },  
 {  
   "id": "b\_r943570",  
   "merchant": "STARBUCKS.123",  
   "amount": 361.76,  
   "ts": "2026-01-12 14:30:13 \+0530"  
 },  
 {  
   "id": "b\_o423300",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 72.18,  
   "ts": "2026-01-12 10:04:04 \+0530"  
 },  
 {  
   "id": "b\_s374160",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 402.97,  
   "ts": "2026-01-12 12:59:35 \+0530"  
 },  
 {  
   "id": "b\_y860028",  
   "merchant": "STARBUCKS.123",  
   "amount": 72.2,  
   "ts": "2026-01-12 12:56:29 \+0530"  
 },  
 {  
   "id": "b\_b849111",  
   "merchant": "LOCAL  GROCERY",  
   "amount": 452.98,  
   "ts": "2026-01-12 15:18:04 \+0530"  
 },  
 {  
   "id": "b\_v458953",  
   "merchant": "UBER BV",  
   "amount": 230.52,  
   "ts": "2026-01-12 15:42:53 \+0530"  
 },  
 {  
   "id": "b\_b885127",  
   "merchant": "Amazon   Marketplace",  
   "amount": 207.41,  
   "ts": "2026-01-12 16:17:49 \+0530"  
 },  
 {  
   "id": "b\_i204843",  
   "merchant": "Ecomm Seller X",  
   "amount": 275.28,  
   "ts": "2026-01-12 12:56:25 \+0530"  
 },  
 {  
   "id": "b\_n999739",  
   "merchant": " starbucks 123 ",  
   "amount": 456.95,  
   "ts": "2026-01-12 11:34:44 \+0530"  
 },  
 {  
   "id": "b\_r492291",  
   "merchant": "UBER, BV",  
   "amount": 75.87,  
   "ts": "2026-01-12 14:12:11 \+0530"  
 },  
 {  
   "id": "b\_p488846",  
   "merchant": "STARBUCKS.123",  
   "amount": 9.03,  
   "ts": "2026-01-12 17:25:55 \+0530"  
 },  
 {  
   "id": "b\_d271348",  
   "merchant": "amzn mktplace",  
   "amount": 146.82,  
   "ts": "2026-01-12 17:33:24 \+0530"  
 },  
 {  
   "id": "b\_d627500",  
   "merchant": "local grocery",  
   "amount": 128.8,  
   "ts": "2026-01-12 15:47:23 \+0530"  
 },  
 {  
   "id": "b\_extra\_w102050",  
   "merchant": "STARBUCKS 123",  
   "amount": 172.74,  
   "ts": "2026-01-12 12:02:37 \+0530"  
 },  
 {  
   "id": "b\_extra\_g142404",  
   "merchant": "Restaurant ABC",  
   "amount": 409.27,  
   "ts": "2026-01-12 15:00:40 \+0530"  
 },  
 {  
   "id": "b\_extra\_a875892",  
   "merchant": "LOCAL GROCERY",  
   "amount": 104.59,  
   "ts": "2026-01-12 10:36:13 \+0530"  
 },  
 {  
   "id": "b\_extra\_d420659",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 398.06,  
   "ts": "2026-01-12 15:30:16 \+0530"  
 },  
 {  
   "id": "b\_extra\_h558132",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 300.82,  
   "ts": "2026-01-12 15:22:35 \+0530"  
 },  
 {  
   "id": "b\_extra\_u736356",  
   "merchant": "Ecomm Seller X",  
   "amount": 272.73,  
   "ts": "2026-01-12 10:23:57 \+0530"  
 },  
 {  
   "id": "b\_extra\_x621340",  
   "merchant": "LOCAL  GROCERY ",  
   "amount": 272.6,  
   "ts": "2026-01-12 12:32:58 \+0530"  
 },  
 {  
   "id": "b\_extra\_p139985",  
   "merchant": " starbucks 123 ",  
   "amount": 62.35,  
   "ts": "2026-01-12 13:37:31 \+0530"  
 },  
 {  
   "id": "b\_extra\_c955372",  
   "merchant": "UBER, BV",  
   "amount": 115.85,  
   "ts": "2026-01-12 11:57:33 \+0530"  
 },  
 {  
   "id": "b\_extra\_i955236",  
   "merchant": "Fuel Station 9",  
   "amount": 244.65,  
   "ts": "2026-01-12 12:53:06 \+0530"  
 },  
 {  
   "id": "b\_extra\_y865919",  
   "merchant": "Local Grocery",  
   "amount": 499.62,  
   "ts": "2026-01-12 10:05:41 \+0530"  
 },  
 {  
   "id": "b\_extra\_l364277",  
   "merchant": "Pharmacy 22",  
   "amount": 93.56,  
   "ts": "2026-01-12 11:24:45 \+0530"  
 },  
 {  
   "id": "b\_extra\_x156301",  
   "merchant": "AMZN MKTPLACE",  
   "amount": 125.38,  
   "ts": "2026-01-12 16:07:41 \+0530"  
 },  
 {  
   "id": "b\_extra\_u191803",  
   "merchant": "amzn mktplace",  
   "amount": 61.61,  
   "ts": "2026-01-12 13:33:27 \+0530"  
 },  
 {  
   "id": "b\_extra\_g343377",  
   "merchant": "amzn mktplace",  
   "amount": 11.68,  
   "ts": "2026-01-12 10:47:38 \+0530"  
 },  
 {  
   "id": "b\_extra\_p872828",  
   "merchant": "Uber  Trip",  
   "amount": 162.63,  
   "ts": "2026-01-12 14:04:00 \+0530"  
 },  
 {  
   "id": "b\_extra\_e270201",  
   "merchant": "Coffee Shop 77",  
   "amount": 360.31,  
   "ts": "2026-01-12 10:18:02 \+0530"  
 },  
 {  
   "id": "b\_extra\_y200295",  
   "merchant": "AMAZON MARKETPLACE",  
   "amount": 182.92,  
   "ts": "2026-01-12 12:42:09 \+0530"  
 },  
 {  
   "id": "b\_extra\_t316763",  
   "merchant": "amzn mktplace",  
   "amount": 336.68,  
   "ts": "2026-01-12 12:11:15 \+0530"  
 },  
 {  
   "id": "b\_extra\_e915146",  
   "merchant": "Fuel Station 9",  
   "amount": 31.1,  
   "ts": "2026-01-12 16:21:24 \+0530"  
 },  
 {  
   "id": "b\_extra\_l402599",  
   "merchant": "NETFLIX.COM",  
   "amount": 142.1,  
   "ts": "2026-01-12 16:12:13 \+0530"  
 },  
 {  
   "id": "b\_extra\_r869595",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 446.99,  
   "ts": "2026-01-12 13:34:24 \+0530"  
 },  
 {  
   "id": "b\_extra\_i759096",  
   "merchant": "Starbucks Store 123",  
   "amount": 189.9,  
   "ts": "2026-01-12 15:51:47 \+0530"  
 },  
 {  
   "id": "b\_extra\_d696310",  
   "merchant": "Amazon   Marketplace",  
   "amount": 242.86,  
   "ts": "2026-01-12 15:18:41 \+0530"  
 },  
 {  
   "id": "b\_extra\_o985689",  
   "merchant": "Coffee Shop 77",  
   "amount": 151.94,  
   "ts": "2026-01-12 12:49:20 \+0530"  
 },  
 {  
   "id": "b\_extra\_h590058",  
   "merchant": " netflix com ",  
   "amount": 339.55,  
   "ts": "2026-01-12 15:50:05 \+0530"  
 },  
 {  
   "id": "b\_extra\_q766572",  
   "merchant": "Amazon Marketplace",  
   "amount": 280.88,  
   "ts": "2026-01-12 17:28:40 \+0530"  
 },  
 {  
   "id": "b\_extra\_a300207",  
   "merchant": "STARBUCKS.123",  
   "amount": 386.82,  
   "ts": "2026-01-12 17:33:26 \+0530"  
 },  
 {  
   "id": "b\_extra\_f824788",  
   "merchant": "NETFLIX",  
   "amount": 191.15,  
   "ts": "2026-01-12 16:47:03 \+0530"  
 },  
 {  
   "id": "b\_extra\_o374135",  
   "merchant": "Local-Grocery",  
   "amount": 6.66,  
   "ts": "2026-01-12 10:17:54 \+0530"  
 },  
 {  
   "id": "b\_dup\_u810087",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 422.38,  
   "ts": "2026-01-12 17:04:04 \+0530"  
 },  
 {  
   "id": "b\_dup\_m888983",  
   "merchant": "Amazon   Marketplace",  
   "amount": 240.45,  
   "ts": "2026-01-12 11:22:40 \+0530"  
 },  
 {  
   "id": "b\_dup\_v229482",  
   "merchant": "UBER, BV",  
   "amount": 115.85,  
   "ts": "2026-01-12 11:57:33 \+0530"  
 },  
 {  
   "id": "b\_dup\_l874819",  
   "merchant": "starbucks 123",  
   "amount": 227.18,  
   "ts": "2026-01-12 13:25:45 \+0530"  
 },  
 {  
   "id": "b\_dup\_o924259",  
   "merchant": "local grocery",  
   "amount": 287.75,  
   "ts": "2026-01-12 12:07:39 \+0530"  
 },  
 {  
   "id": "b\_dup\_m226537",  
   "merchant": "AMZN. MKTPLACE",  
   "amount": 166.47,  
   "ts": "2026-01-12 13:48:23 \+0530"  
 },  
 {  
   "id": "b\_dup\_j700889",  
   "merchant": "Uber  Trip",  
   "amount": 193.25,  
   "ts": "2026-01-12 15:27:09 \+0530"  
 },  
 {  
   "id": "b\_dup\_p938258",  
   "merchant": "local grocery",  
   "amount": 115.14,  
   "ts": "2026-01-12 15:59:10 \+0530"  
 },  
 {  
   "id": "b\_dup\_w640750",  
   "merchant": "NETFLIX,COM",  
   "amount": 212.45,  
   "ts": "2026-01-12 14:04:15 \+0530"  
 },  
 {  
   "id": "b\_dup\_d443444",  
   "merchant": "netflix com",  
   "amount": 346.8,  
   "ts": "2026-01-12 14:09:38 \+0530"  
 },  
 {  
   "id": "b\_dup\_e862049",  
   "merchant": "amzn mktplace",  
   "amount": 281.02,  
   "ts": "2026-01-12 13:31:29 \+0530"  
 },  
 {  
   "id": "b\_dup\_e729054",  
   "merchant": "netflix com",  
   "amount": 457.61,  
   "ts": "2026-01-12 16:21:07 \+0530"  
 },  
 {  
   "id": "b\_dup\_r241264",  
   "merchant": "Coffee Shop 77",  
   "amount": 341.64,  
   "ts": "2026-01-12 11:03:43 \+0530"  
 },  
 {  
   "id": "b\_dup\_d321243",  
   "merchant": "Local Grocery",  
   "amount": 103.98,  
   "ts": "2026-01-12 14:10:57 \+0530"  
 },  
 {  
   "id": "b\_dup\_x632724",  
   "merchant": "Amazon   Marketplace",  
   "amount": 254.94,  
   "ts": "2026-01-12 12:10:49 \+0530"  
 }  
\]

# merchant\_map.json

{  
 "AMZN MKTPLACE": "Amazon",  
 "Amazon Marketplace": "Amazon",  
 "AMAZON MARKETPLACE": "Amazon",  
 "STARBUCKS 123": "Starbucks",  
 "Starbucks Store 123": "Starbucks",  
 "UBER BV": "Uber",  
 "Uber Trip": "Uber",  
 "NETFLIX": "Netflix",  
 "Netflix.com": "Netflix",  
 "LOCAL GROCERY": "Local Grocery",  
 "Local Grocery": "Local Grocery"  
}  
