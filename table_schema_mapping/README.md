# Multi-Client Multi-Table Data Standardization Pipeline

## 📌 Problem Statement

We work with **multiple clients**, each providing **multiple tables**, and every client names their columns differently even though their tables contain **similar types of data**.  
We also have a **data dictionary** that defines the correct column names and structure for the **destination standardized tables**.

The goal is to:

- Take all input tables from all clients  
- Map each of their columns to standardized column names using JSON mapping files  
- Automatically group all client tables that map to the same destination table  
- Generate clean, unified **destination tables**  

The pipeline must support **any number of clients** and **any number of source tables**, while keeping code changes minimal.

---

## 📌 Project Overview

This project implements a **dynamic, scalable, configuration-driven ETL pipeline** that automatically standardizes data from multiple clients according to a data dictionary.

Key features:

- Unlimited clients  
- Unlimited tables  
- JSON-based column mapping for each client  
- Automatic destination table detection  
- Single unified pipeline – no code updates required when clients/tables increase  
- Fully driven by folder structure + JSON configs  

The only thing to update when clients/tables increase is the **clients dictionary**, which maps client IDs to:
- Their JSON mapping file  
- Their source tables  

All other logic stays completely unchanged.

---

## 📁 Recommended Folder Structure

```
project/
│
├── Client Files/
│     ├── c01t01.csv
│     ├── c01t02.csv
│     ├── c01t03.csv
│     ├── c02t01.csv
│     ├── c02t02.csv
│     ├── c02t03.csv
│     └── ...
│
├── Client Json/
│     ├── client01.json
│     ├── client02.json
│     └── ...
│
├── Output Files/
│     └── (Generated standardized tables)
│
├── Client Files/data_dictionary.csv
│
└── mapping_n_number.ipynb
```

---

## 📌 How the Pipeline Works

1. Load the **data dictionary** to build the expected destination tables  
2. Load all client mapping JSON files  
3. Create a `clients` dictionary (this is the only part you update when adding/removing clients or tables)  
4. For each client:
   - Read all their CSV tables  
   - Use their JSON mapping to standardize columns  
   - Automatically detect which destination table each source table should go to  
   - Append rows with a `client` identifier  
5. Output clean standardized tables into `/Output Files`

---

## 📌 Adding or Removing Clients/Tables

This pipeline is **designed for flexible scaling**.

To add a new client:
1. Add their CSV files into `/Client Files`
2. Add their JSON mapping in `/Client Json`
3. Add them in the `clients` dictionary in the notebook

To add/remove tables:
- Adjust the client’s JSON mapping file  
- Add/remove entries from that client's section in the `clients` dictionary  

**No core processing logic needs modification.  
The ETL code stays the same forever.**

---

## 📌 Outputs

Example output directory:

```
Output Files/
│── ft01.csv
│── ft02.csv
└── ft03.csv
```

Each file contains:

- Standardized column names from the data dictionary  
- Merged rows from all clients  
- A `client` column identifying the source client  

---

## 📌 Summary

This project provides a robust and scalable system for standardizing multi-client, multi-table datasets using:

- A data dictionary  
- JSON mapping files  
- Dynamic table-building logic  
- Automatic destination-table selection  
- A simple configurable client dictionary  

Only one small update is ever needed (the clients dictionary), making the entire pipeline extremely easy to maintain and expand.
