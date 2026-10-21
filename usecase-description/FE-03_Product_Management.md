# MODULE FE-03: PRODUCT CATALOG MANAGEMENT

---

### UC-08 Manage Categories and Brands

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to manage a hierarchical category structure and sanitary ware brand profiles (TOTO, Inax, Caesar, Viglacera, American Standard) so that bathroom fixtures are organized consistently for browsing and inventory management.  

**Preconditions**  
1. The Store Owner is logged in and authorized to manage product classifications.  
2. The Product Categories & Brands screen is accessible in the Admin Portal.  

**Normal Flow**  
1. The Store Owner opens the Product Categories & Brands screen.  
2. The system displays a hierarchical tree of parent and child categories (e.g., Toilets -> Smart Toilets, One-piece Toilets, Wall-hung Toilets; Washbasins -> Cabinet Basins, Countertop Basins; Showers & Faucets; Bathtubs; Bathroom Accessories) alongside a Brands management tab.  
3. Each category node displays its Code, Name, Icon, linked product count, and Visibility status (`Visible`/`Hidden`).  
4. The Store Owner expands or collapses branches to inspect the category hierarchy.  
5. The Store Owner drags a category node to rearrange its display position.  
6. The system updates and saves the display order in the database.  
7. The system refreshes the tree and displays MSG134: "Category display order updated successfully."  

**Alternative Flows**  
- **Step 2 - No Categories Available**:  
  1. The system displays an empty tree and a "Create First Root Category" button.  
  2. The Store Owner clicks the button.  
  3. The system opens a category creation modal.  
  4. The Store Owner enters Category Name, Code, selects an icon, and clicks "Save".  
  5. The system creates the root category and reloads the tree.  
- **Step 3 - Create New Child Category**:  
  1. The Store Owner clicks "Add Subcategory" on an existing category node (e.g., under "Toilets").  
  2. The system displays the form requesting Name (e.g., "Wall-hung Toilets"), Code (`CAT-TOI-WH`), Description, and Category Banner.  
  3. The Store Owner uploads a banner image to Cloudinary and saves.  
  4. The system links the subcategory under the parent and displays MSG30: "Category created successfully."  
- **Step 3 - Update or Toggle Visibility**:  
  1. The Store Owner edits category metadata or toggles Visibility to `Hidden`.  
  2. The system persists changes and displays MSG31: "Category updated successfully."  
- **Step 3 - Delete Category with Validation**:  
  1. The Store Owner clicks "Delete" on a category.  
  2. The system verifies if any active products or child subcategories are linked.  
  3. If products are linked: The system blocks deletion and displays MSG32: "Cannot delete category containing active products. Reassign or delete products first."  
  4. If empty: The system prompts for confirmation, soft-deletes the category, and displays MSG33: "Category deleted successfully."  
- **Step 2 - Manage Brands Sub-flow**:  
  1. The Store Owner clicks the "Brands" tab.  
  2. The system displays a grid of sanitary ware brands (TOTO, Inax, Viglacera, etc.) showing Brand Logo, Origin (Japan, Vietnam, Germany), Website, and Status.  
  3. The Store Owner adds or updates a brand with a logo uploaded to Cloudinary.  
  4. The system saves the brand record and displays MSG34: "Brand updated successfully."  

**Postconditions**  
1. Category tree hierarchy, ordering, and brand definitions are stored in the database.  
2. Catalog navigation menus and filter bars reflect the updated categories and brands immediately.  

---

### UC-09 Manage Products

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to create, update, and manage master sanitary ware products with technical specifications, origin, installation guides, and gallery media so that bathroom fixtures are accurately presented for sale.  

**Preconditions**  
1. The Store Owner is authenticated with catalog management permissions.  
2. Relevant Categories (UC-08) and Brands have been created.  

**Normal Flow**  
1. The Store Owner navigates to the "Product Management" list in the Admin Portal.  
2. The system displays a paginated list of sanitary ware products showing Product Code, Image, Name, Category, Brand, Total Variants, Base Price Range, and Status (`Draft`, `Active`, `Discontinued`).  
3. The Store Owner clicks "Create Product".  
4. The system opens the Product Creation form with sections:  
   - Basic Info: Product Name (e.g., "TOTO Neorest Smart Toilet"), Product Code (`PROD-TOTO-NEO-01`), Category, Brand, Origin.  
   - Detailed Description & Rich-Text Features (CeFiONtect ceramic glaze, Tornado flush, auto-deodorizer).  
   - Technical Specifications (Rough-in dimension 305mm, Water pressure 0.05-0.75 MPa, Power rating 220V/850W, Trapway S-Trap).  
   - Media Gallery: Upload high-resolution product photos, dimensional blueprints, and installation specification PDFs.  
5. The Store Owner fills in all required fields and uploads media files.  
6. The system uploads image assets and specification drawings to Cloudinary and retrieves secure asset URLs.  
7. The Store Owner sets initial status to `Active` and clicks "Save Product".  
8. The system validates uniqueness of the Product Code, saves the master product, and displays MSG35: "Product created successfully. Please configure SKUs and variants."  

**Alternative Flows**  
- **Step 2 - Search, Filter, and Edit Product**:  
  1. The Store Owner filters products by Category, Brand, or searches by keyword.  
  2. The Store Owner clicks "Edit" on a product.  
  3. The system loads product data into the form.  
  4. The Store Owner modifies details (e.g., updates technical specs or replaces gallery images) and clicks "Save Changes".  
  5. The system saves changes and displays MSG36: "Product details updated successfully."  
- **Step 4 - Duplicate Product Code**:  
  1. The entered Product Code already exists in the system.  
  2. The system flags the field with MSG37: "Product Code must be unique. This code is already in use."  
  3. The Store Owner enters an alternate code.  
- **Step 2 - Discontinue / Archive Product**:  
  1. The Store Owner changes product status to `Discontinued`.  
  2. The system prompts: "Discontinuing this product will hide it from the customer storefront while preserving existing sales history."  
  3. The Store Owner confirms. The system updates status and displays MSG38: "Product marked as Discontinued."  

**Postconditions**  
1. Master product record and technical attributes are persisted.  
2. Media asset URLs are stored and ready for storefront and quote PDF rendering.  

---

### UC-10 Manage SKUs, Variants, and Pricing

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to manage individual product variants, barcodes/SKUs, dimensions, finishes, retail prices, and wholesale pricing tiers so that inventory and sales operations can accurately reference specific saleable items.  

**Preconditions**  
1. The master product container has been created (UC-09).  
2. The Store Owner is on the "Variants & SKUs" tab of the product editor.  

**Normal Flow**  
1. The Store Owner opens the "Variants & Pricing" section for a product (e.g., "TOTO Neorest Smart Toilet").  
2. The system displays existing variant rows: SKU Code, Barcode, Variant Name (e.g., "White - Rough-in 305mm", "Matte Black - Rough-in 305mm"), Attributes (Color, Rough-in, Flushing Volume), Retail Selling Price, Contractor/Wholesale Price, and Cost Price.  
3. The Store Owner clicks "Add Variant".  
4. The system opens a variant configuration row/form.  
5. The Store Owner enters:  
   - SKU Code (e.g., `SKU-NEO-WHT-305`).  
   - Barcode / EAN-13.  
   - Variant Attributes: Color (`Glossy White`), Dimensions (`690 x 390 x 510 mm`), Weight (`45 kg`).  
   - Cost Price (e.g., `45,000,000 VND`).  
   - Retail Selling Price (e.g., `68,000,000 VND`).  
   - Wholesale / Project Tier Price (e.g., `58,000,000 VND`).  
6. The Store Owner clicks "Save Variants".  
7. The system validates that SKU Code and Barcode are globally unique across all products.  
8. The system saves the variant records and automatically initializes inventory ledger rows (with stock quantity 0) across all warehouses.  
9. The system displays MSG39: "SKU and variant pricing saved successfully."  

**Alternative Flows**  
- **Step 2 - Bulk Generate Variants via Attributes Matrix**:  
  1. The Store Owner clicks "Generate Variants Matrix".  
  2. The Store Owner selects Attribute 1 (Color: Chrome, Matte Black, Brushed Gold) and Attribute 2 (Spout Length: 150mm, 200mm).  
  3. The system generates all 6 combinations and pre-populates SKU templates.  
  4. The Store Owner enters pricing for each combination and clicks "Confirm".  
  5. The system batch-creates the variants and displays MSG40: "Generated 6 variants successfully."  
- **Step 7 - Duplicate SKU or Barcode**:  
  1. The entered SKU code or Barcode already exists in another product.  
  2. The system rejects saving and highlights the duplicate row with MSG41: "Duplicate SKU or Barcode detected. Each variant must have unique identifiers."  
- **Step 5 - Invalid Price Relationship**:  
  1. The Store Owner sets Retail Price lower than Cost Price without override authorization.  
  2. The system warns: "Selling price is lower than cost price. Are you sure you want to proceed?"  
  3. The Store Owner acknowledges or adjusts the price.  

**Postconditions**  
1. Distinct SKU variant records are created with pricing tiers.  
2. Inventory tracking records are initialized for the new SKUs.  

---

### UC-11 Import and Export Product Data

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to bulk import sanitary ware products and SKUs using standardized Excel templates and export catalog datasets so that large-scale product catalogs can be rapidly initialized, audited, and maintained.  

**Preconditions**  
1. The Store Owner is authenticated and has catalog import/export permissions.  
2. The "Import / Export Products" tool is accessible in the Admin Portal.  

**Normal Flow**  
1. The Store Owner navigates to the "Product Import / Export" page.  
2. The Store Owner clicks "Download Excel Template" to obtain the standardized spreadsheet schema (containing columns for Product Code, Name, Category Code, Brand, SKU, Attributes, Cost Price, Selling Price, Specs).  
3. The Store Owner fills in product rows in the Excel file offline.  
4. The Store Owner clicks "Upload File" and selects the completed `.xlsx` file.  
5. The system parses the file and performs pre-validation:  
   - Validates column headers against schema.  
   - Verifies referenced Categories and Brands exist.  
   - Checks for duplicate SKUs within the file and against existing database records.  
   - Validates data formats (numeric prices, non-empty mandatory fields).  
6. The system displays a Preview Grid showing Total Rows, Valid Rows (e.g., 150), and Error Rows (0).  
7. The Store Owner clicks "Execute Import".  
8. The system batch-inserts products and SKU variants in a transactional database operation.  
9. The system logs the bulk import event in the Activity Log.  
10. The system displays MSG42: "Import completed successfully: 150 products and SKUs created/updated."  

**Alternative Flows**  
- **Step 5 - Validation Errors in Import File**:  
  1. The system detects validation errors (e.g., Category Code does not exist, invalid price format on Row 14 and Row 28).  
  2. The system flags the invalid rows with explicit error messages.  
  3. The system allows the Store Owner to download an Error Report spreadsheet containing highlighted issues.  
  4. The Store Owner corrects the spreadsheet and re-uploads.  
- **Step 1 - Export Products Sub-flow**:  
  1. The Store Owner filters products by Category or Brand and clicks "Export to Excel".  
  2. The system compiles all matching master products, variants, and pricing tiers into a formatted `.xlsx` workbook.  
  3. The system initiates browser download of the file.  

**Postconditions**  
1. Master products and variants are imported or updated in bulk.  
2. An import summary log entry is created with row counts and user ID.  

---

### UC-12 Search and View Product Information

**Primary Actors**  
Guest, Customer, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a user or sales staff, I want to search and filter sanitary ware products by category, brand, price range, technical specifications, and real-time stock availability so that I can evaluate fixtures, verify compatibility, and check inventory.  

**Preconditions**  
1. The AquaSuite storefront or POS Product Search interface is loaded.  

**Normal Flow**  
1. The user navigates to the Product Catalog or enters a search term in the search bar (e.g., "TOTO smart toilet 305mm").  
2. The system performs full-text search and renders the product catalog grid with faceted filter sidebars:  
   - Category (Toilets, Basins, Showers, Bathtubs).  
   - Brand (TOTO, Inax, Caesar, Viglacera).  
   - Price Range slider.  
   - Technical Specifications (Flushing type, Rough-in dimension, Ceramic glaze technology).  
   - Availability toggle ("In Stock Only").  
3. The user applies filters (e.g., Brand = "TOTO", Category = "Smart Toilets", In Stock = Yes).  
4. The system refreshes results instantly, displaying product cards with thumbnail image, product name, brand badge, starting price, and stock status badge (`In Stock` / `Pre-order`).  
5. The user clicks on a product card to view the Product Details page.  
6. The system displays the comprehensive product page:  
   - Image gallery with zoom capabilities and technical CAD blueprints.  
   - Variant selector (Color, Rough-in dimension) dynamically updating SKU price and stock count.  
   - Real-time stock status (For Sales Staff: warehouse-level breakdown; For Customer: general availability).  
   - Complete technical specifications table and downloadable installation manual PDF.  
   - Warranty terms (e.g., 10-year ceramic warranty, 2-year electronic valve warranty).  

**Alternative Flows**  
- **Step 2 - No Products Found**:  
  1. No products match the search query or combined filters.  
  2. The system displays MSG43: "No products found matching your criteria. Try adjusting your filters or search keywords."  
  3. The system provides links to top popular categories (Smart Toilets, Washbasins).  
- **Step 6 - Out of Stock Variant Selected**:  
  1. The user selects a variant currently with zero inventory.  
  2. The system displays a "Temporarily Out of Stock - Contact Sales for Reservation" badge and disables the "Instant Buy" button while enabling "Notify Me When Available" or "Add to Quote".  

**Postconditions**  
1. Product views and search events are tracked for analytics.  
2. Selected product details are loaded for cart addition or quote generation.  

---

### UC-13 View and Manage Product Combos

**Primary Actors**  
Store Owner, Sales Staff, Customer, Guest  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, Sales Staff, or Customer, I want to configure and browse packaged bathroom fixture combos (e.g., Complete Master Bathroom Suite: Smart Toilet, Vanity Sink, Thermostatic Shower Set, and 6-piece Accessory Kit) with bundled package discounts so that customers can purchase coordinated bathroom sets at favorable pricing.  

**Preconditions**  
1. For Store Owner: Logged in with catalog permissions to create/edit combos.  
2. For Customer/Sales: Accessing the "Bathroom Combos & Bundles" section.  

**Normal Flow**  
1. The Store Owner navigates to the "Product Combos & Packages" manager in the Admin Portal.  
2. The system displays active combo packages with Package Name, Thumbnail, Linked Items Count, Original Total Price, Package Special Price, and Discount %.  
3. The Store Owner clicks "Create Combo Package".  
4. The system opens the combo creation modal.  
5. The Store Owner enters Combo Title (e.g., "Luxury Master Suite Package - TOTO Neorest"), description, and uploads package promotional banner to Cloudinary.  
6. The Store Owner searches and adds constituent SKU items to the bundle:  
   - 1x TOTO Neorest Smart Toilet (Retail: 68,000,000 VND).  
   - 1x Countertop Basin Sink (Retail: 12,000,000 VND).  
   - 1x Tall Basin Mixer Tap (Retail: 6,500,000 VND).  
   - 1x Thermostatic Rain Shower System (Retail: 18,500,000 VND).  
   - 1x 6-piece Solid Brass Accessory Set (Retail: 5,000,000 VND).  
7. The system computes total standard retail sum: `110,000,000 VND`.  
8. The Store Owner sets Package Promotional Price: `95,000,000 VND` (Saving: 15,000,000 VND / 13.6%).  
9. The Store Owner sets validity dates and stock reservation rules (whether combo checks real-time component SKU inventory).  
10. The Store Owner clicks "Publish Combo".  
11. The system saves the combo package and publishes it to the storefront and POS quote builder with MSG44: "Bathroom package created successfully."  

**Alternative Flows**  
- **Step 2 - Customer Browses and Purchases Combo**:  
  1. A Customer navigates to "Bathroom Packages" on the website.  
  2. The system renders the bundled fixtures with high-resolution bathroom layout imagery.  
  3. The Customer selects component options (e.g., choosing Chrome or Matte Black finish for shower/faucets).  
  4. The Customer clicks "Add Complete Package to Cart".  
  5. The system adds all constituent SKUs with package discount applied to the cart.  
- **Step 6 - Component SKU Depleted in Warehouse**:  
  1. One of the mandatory SKUs inside a combo runs out of stock in the warehouse.  
  2. The system flags the combo as "Partially Out of Stock" on the storefront, showing which fixture is depleted and offering similar substitute SKUs or pre-order reservation.  

**Postconditions**  
1. The combo package definition and component mappings are stored in the database.  
2. Combo pricing is recognized across POS sales, project quotations, and storefront checkout.  
