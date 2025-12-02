# How to Upload Rug Photos to This Repository

## Quick Start

This repository is set up to organize your Antique Rugs Crafts product listings and images. Follow these steps to add your rug photos.

## Repository Structure

```
antique-rugs-crafts/
├── README.md
├── UPLOAD_GUIDE.md
├── products/
│   ├── product-listings.csv
│   ├── hand-tufted/
│   │   ├── rug_001.jpg
│   │   └── rug_003.jpg
│   └── flatweave/
│       ├── rug_002.jpg
│       └── rug_004.jpg
└── docs/
    └── SEO-KEYWORDS.md
```

## Uploading Photos via GitHub Web Interface

### Method 1: Upload via Browser

1. Navigate to your repository: `https://github.com/JokerAlig/antique-rugs-crafts`
2. Click **"Add file"** button
3. Select **"Upload files"**
4. Drag and drop your rug photos or select them from your computer
5. Add commit message (e.g., "Add hand-tufted rug photos")
6. Click **"Commit changes"**

### Photo Organization Tips

- Create folders by type: `hand-tufted/`, `flatweave/`, `kilim/`, etc.
- Name files clearly: `rug_RUG001.jpg`, `rug_RUG002.jpg`
- Use descriptive names: `kashmir-blue-wool-5x7.jpg`
- Supported formats: JPG, PNG, WebP
- Recommended size: 1200x800px or larger for e-commerce

### SEO Optimization for Images

When uploading, pair each image with metadata:

- **Filename**: Use keywords (e.g., `hand-tufted-kashmiri-blue-wool-rug.jpg`)
- **Alt text**: Add descriptive alt text to CSV file
- **Product ID**: Reference product ID from `product-listings.csv`

## CSV Template for Product Listings

The `products/product-listings.csv` file includes columns for:

- ProductID
- Name
- Type (Hand-Tufted, Flatweave, Kilim, etc.)
- Size
- Price
- AltText (for images)
- Description (SEO-optimized)
- Keywords (comma-separated)
- SKU

## Using Git Command Line (Advanced)

```bash
# Clone the repository
git clone https://github.com/JokerAlig/antique-rugs-crafts.git
cd antique-rugs-crafts

# Create a new folder for photos
mkdir products/my-rugs
cd products/my-rugs

# Add your photos
cp /path/to/rug_photos/*.jpg .

# Commit and push
git add .
git commit -m "Add new rug photo collection"
git push origin main
```

## Tips for E-Commerce

1. **High-Quality Images**: Use professional photos with good lighting
2. **Multiple Angles**: Show top, side, and detail views
3. **Consistent Naming**: Use your SKU system in filenames
4. **Metadata**: Keep the CSV updated with all product details
5. **SEO Keywords**: Include trending keywords for global buyers

## Next Steps

- Update `products/product-listings.csv` with your actual rug inventory
- Create category folders inside `products/`
- Add high-resolution photos
- Use this data for listings on eBay, Flipkart, Amazon, Pinterest

## Questions?

Refer to GitHub documentation: https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository
