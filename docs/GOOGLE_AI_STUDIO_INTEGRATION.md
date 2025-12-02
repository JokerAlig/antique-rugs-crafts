# Using GitHub Photos with Google AI Studio
## Integration Guide for Antique Rugs Crafts App

## Overview

This guide shows you how to:
1. Upload rug photos to this GitHub repository
2. Access the raw image URLs from GitHub
3. Use those images in Google AI Studio for your rug app

---

## Step 1: Upload Your Rug Photos to GitHub

### Method A: Using GitHub Web Interface

1. Go to your repository: `https://github.com/JokerAlig/antique-rugs-crafts`
2. Click **"Add file"** → **"Upload files"**
3. Drag & drop your rug photos or click "choose your files"
4. Create folder by typing in filename: `products/hand-tufted/rug-001.jpg`
5. Add commit message: "Add rug photos for app"
6. Click **"Commit changes"**

### Supported Image Formats
- JPG/JPEG
- PNG
- WebP
- Recommended size: 1200x800px minimum (high resolution for AI)

### Folder Organization Example
```
products/
├── hand-tufted/
│   ├── rug-001-blue-kashmir.jpg
│   ├── rug-002-maroon-persian.jpg
│   └── rug-003-traditional.jpg
├── flatweave/
│   ├── rug-004-beige-cotton.jpg
│   └── rug-005-geometric.jpg
└── kilim/
    └── rug-006-colorful-kilim.jpg
```

---

## Step 2: Get Raw Image URLs from GitHub

### Getting the Raw URL

Once your image is uploaded, you need the **RAW** URL:

1. Click on the image file in GitHub
2. Click **"View raw"** button (top right of image)
3. Copy the URL from the address bar

### URL Format

**Normal GitHub URL:**
```
https://github.com/JokerAlig/antique-rugs-crafts/blob/main/products/hand-tufted/rug-001.jpg
```

**RAW URL (for API/Apps):**
```
https://raw.githubusercontent.com/JokerAlig/antique-rugs-crafts/main/products/hand-tufted/rug-001.jpg
```

**Important:** Always use the **raw.githubusercontent.com** URL for your app!

---

## Step 3: Store URLs in a JSON File

### Create a Product Images JSON File

Create a file: `products/images-manifest.json`

```json
{
  "rugs": [
    {
      "id": "RUG001",
      "name": "Hand-Tufted Kashmir Blue Wool Rug",
      "type": "hand-tufted",
      "imageUrl": "https://raw.githubusercontent.com/JokerAlig/antique-rugs-crafts/main/products/hand-tufted/rug-001-blue-kashmir.jpg",
      "altText": "Luxurious hand-tufted Kashmir blue wool rug",
      "description": "Premium hand-tufted Kashmir blue wool rug, 5x7 feet"
    },
    {
      "id": "RUG002",
      "name": "Flatweave Beige Cotton Rug",
      "type": "flatweave",
      "imageUrl": "https://raw.githubusercontent.com/JokerAlig/antique-rugs-crafts/main/products/flatweave/rug-004-beige-cotton.jpg",
      "altText": "Casual flatweave beige cotton rug",
      "description": "Versatile flatweave beige cotton rug, 4x6 feet"
    }
  ]
}
```

---

## Step 4: Integrate with Google AI Studio

### Option A: Using Image URLs Directly

In Google AI Studio, you can pass image URLs:

```javascript
// JavaScript/Node.js Example
const imageUrl = "https://raw.githubusercontent.com/JokerAlig/antique-rugs-crafts/main/products/hand-tufted/rug-001.jpg";

const response = await model.generateContent([
  {
    inlineData: {
      mimeType: "image/jpeg",
      data: await fetch(imageUrl).then(r => r.arrayBuffer())
    }
  },
  "Describe this rug's features and quality"
]);
```

### Option B: Using Base64 Encoding (Recommended for Apps)

1. Fetch image from GitHub
2. Convert to Base64
3. Send to Gemini API

```javascript
// Convert image URL to Base64
async function urlToBase64(imageUrl) {
  const response = await fetch(imageUrl);
  const blob = await response.blob();
  return new Promise((resolve) => {
    const reader = new FileReader();
    reader.onloadend = () => resolve(reader.result.split(',')[1]);
    reader.readAsDataURL(blob);
  });
}

// Use with Gemini
const imageUrl = "https://raw.githubusercontent.com/JokerAlig/antique-rugs-crafts/main/products/hand-tufted/rug-001.jpg";
const base64Image = await urlToBase64(imageUrl);

const response = await model.generateContent([
  {
    inlineData: {
      mimeType: "image/jpeg",
      data: base64Image
    }
  },
  "Analyze this rug product image"
]);
```

### Option C: Using Flask/Express Backend

**Python Flask Example:**
```python
import requests
from base64 import b64encode

image_url = "https://raw.githubusercontent.com/JokerAlig/antique-rugs-crafts/main/products/hand-tufted/rug-001.jpg"
response = requests.get(image_url)
base64_image = b64encode(response.content).decode('utf-8')
```

**JavaScript Express Example:**
```javascript
const fetch = require('node-fetch');

const imageUrl = "https://raw.githubusercontent.com/JokerAlig/antique-rugs-crafts/main/products/hand-tufted/rug-001.jpg";
const response = await fetch(imageUrl);
const buffer = await response.buffer();
const base64 = buffer.toString('base64');
```

---

## Step 5: Sample Google AI Studio Prompts

### For Rug Analysis

```
Analyze this rug image and provide:
1. Rug type (hand-tufted, flatweave, kilim, etc.)
2. Estimated color scheme
3. Pattern description
4. Apparent quality indicators
5. Suggested product description (100 words)
6. SEO keywords (5-10 keywords)
```

### For Product Listing Generation

```
Based on this rug image, generate:
1. Product title
2. Product description (150 words, SEO-optimized)
3. Key features (bullet points)
4. Material and care instructions
5. Target buyer profile
6. Suggested price range (for reference)
```

---

## Complete Integration Example

### HTML App Example

```html
<!DOCTYPE html>
<html>
<head>
    <title>Antique Rugs Crafts - AI Analyzer</title>
</head>
<body>
    <h1>Rug Product Analyzer</h1>
    <input type="text" id="imageUrl" placeholder="Paste GitHub raw image URL">
    <button onclick="analyzeRug()">Analyze Rug</button>
    <div id="result"></div>

    <script>
    async function analyzeRug() {
        const imageUrl = document.getElementById('imageUrl').value;
        
        // Convert to base64
        const response = await fetch(imageUrl);
        const blob = await response.blob();
        const reader = new FileReader();
        
        reader.onloadend = async () => {
            const base64 = reader.result.split(',')[1];
            
            // Send to Gemini API (you need API key)
            const apiKey = 'YOUR_API_KEY';
            const geminiResponse = await fetch(
                `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${apiKey}`,
                {
                    method: 'POST',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({
                        contents: [{
                            parts: [
                                {
                                    inlineData: {
                                        mimeType: 'image/jpeg',
                                        data: base64
                                    }
                                },
                                {
                                    text: "Analyze this rug and provide: 1) Type, 2) Colors, 3) Pattern, 4) Quality, 5) SEO-optimized description"
                                }
                            ]
                        }]
                    })
                }
            );
            
            const data = await geminiResponse.json();
            document.getElementById('result').innerHTML = data.candidates[0].content.parts[0].text;
        };
        
        reader.readAsDataURL(blob);
    }
    </script>
</body>
</html>
```

---

## Troubleshooting

### Image Not Loading
- Verify you're using **raw.githubusercontent.com** URL
- Check file is actually uploaded to GitHub
- Try viewing image directly in browser first

### CORS Issues
- GitHub raw content usually has CORS enabled
- If issues persist, use a backend server to proxy the image

### File Size Limits
- GitHub has 100MB file limit per file
- For videos/very large images, use GitHub Releases instead

---

## Best Practices

1. **Organize images** in logical folders (by type, collection, etc.)
2. **Use descriptive filenames** with product IDs
3. **Maintain a manifest JSON** with all image URLs and metadata
4. **Test URLs** before deploying to production
5. **Keep product-listings.csv** and images manifest in sync
6. **Version control** - commit with meaningful messages

---

## Next Steps

1. Upload your rug photos to `products/` folder
2. Create `products/images-manifest.json` with URLs
3. Test URLs in your app
4. Integrate with Google AI Studio
5. Generate product descriptions automatically
6. Push generated content to your e-commerce platforms
