# Data Mapping Documentation - Ounass UAE

This document explains how the data fields in the output file (`ounass_product_details.csv`) are mapped from the source website.

**Source**: `https://www.ounass.ae` Product Pages
**Method**: We prioritize **Structured Data (JSON-LD)** found in the `<script type="application/ld+json">` tag. If that is missing or incomplete, we fallback to scraping specific HTML elements.

| Output Field | JSON-LD Path (Preferred) | HTML Fallback (Selector) | Description |
| :--- | :--- | :--- | :--- |
| `name` | `name` | `h1.ProductName` | The full name of the product. |
| `brand` | `brand.name` OR `brand` | `a.BrandName` OR `h2.BrandName` | The brand/designer of the product. |
| `price` | `offers.price` OR `offers[0].price` | `span.Price` OR `span.RegularPrice` | The extraction price (numeric). |
| `currency` | `offers.priceCurrency` | *(Not explicitly scraped in HTML fallback)* | The currency code (e.g., AED). |
| `image_url` | `image` | *(None)* | URL of the main product image. |
| `sku` | `sku` | *(None)* | Unique product identifier (SKU). |
| `description`| `description` | *(None)* | Detailed product description. |

## Notes on Logic
1.  **JSON-LD First**: The script first parses the page for a JSON object with `"@type": "Product"`. If found, it extracts all available fields from there. This is the most reliable method as it relies on the machine-readable data the site provides to Google.
2.  **HTML Fallback**: If JSON-LD fails, the script looks for visible elements on the page (like the `<h1>` title or `span.Price`). This is prone to breaking if the site design changes.
