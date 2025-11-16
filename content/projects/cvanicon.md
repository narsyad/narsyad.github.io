---
title: CV Anicon
slug: cvanicon
description: Landing Page for CV Anicon
longDescription: ""
cardImage: "https://github.com/narsyad/cv-anicon"
tags: ["front-end","nextjs","reactjs","typescript","tailwindcss"]
githubUrl: https://github.com/narsyad/cv-anicon
timestamp: 2025-02-24T02:39:03+00:00
featured: true
---
## Key Features

The application provides the following core features:

1. **Product Catalog System** - Displays precast concrete products (U-ditch, Box Culvert, Barriers, Pipes, etc.) with detailed specifications and image carousels  

2. **Dynamic Product Details** - Individual product pages with conditional rendering based on product ID, showing specifications and WhatsApp ordering integration  
3. **Company Information** - About page with ISO certifications (ISO 9001:2015 & ISO 45001:2018) and project references <cite />

4. **Dark/Light Theme Support** - Theme switching capability with default dark theme  

5. **Responsive Design** - Mobile-first responsive grid layouts with breakpoints for different screen sizes  

6. **Company Values Display** - Features section highlighting commitment, experience, trustworthiness, quality standards, partnership, and fabrication capabilities  

## Tech Stack

**Frontend Framework:**
- Next.js 13+ with App Router architecture  
- TypeScript for type safety  
- React (via Next.js)

**Styling:**
- Tailwind CSS for utility-first styling  
- PostCSS with Autoprefixer  
- Custom color scheme with primary green (#019342)  
- Inter font family from Google Fonts  

**State Management:**
- next-themes for theme management  

**Image Handling:**
- Next.js Image component with Sanity.io CDN support  

## Core Logic & Module Functions

**1. Product Display System**

The product system uses a data-driven approach with `blogData` array containing 12 product entries. Products are rendered conditionally based on ID:

- **Product List** (`app/product/page.tsx`) - Maps through `blogData` to display all products in a responsive grid <cite />
- **Product Details** (`app/blog-details/[id]/page.tsx`) - Uses dynamic routing with `[id]` parameter to fetch and display individual product data  

**2. Conditional Component Rendering**

The product detail page implements two-tier conditional rendering:
- **Carousel Components** (IDs 1-8) - Display product image galleries  
- **Detail Components** (IDs 1-5 only) - Show comprehensive specifications for select products  

**3. WhatsApp Integration**

Direct ordering functionality via WhatsApp with pre-populated message template  

**4. Features Module**

Displays company values in a grid layout by mapping through `featuresData` array containing 6 feature items  

**5. Theme Provider**

Wraps the application with ThemeProvider for dark/light mode switching  

## Architecture Overview

**Routing Structure:**
```
app/
├── page.tsx                    (Home - /)
├── about/page.tsx             (About - /about)
├── product/page.tsx           (Product List - /product)
└── blog-details/[id]/page.tsx (Product Detail - /blog-details/:id)
```
<cite />

**Component Organization:**
- Modular component structure with reusable UI elements
- Feature-based organization (Features, Blog, Contact, Pricing, etc.)
- Shared components in `components/Common/`
- Type definitions in `@/types/` directory

**Data Flow:**
1. Static product data stored in `blogData` array
2. Product list page renders all products from data array
3. Dynamic product detail pages fetch data by ID parameter
4. Conditional rendering determines which components display based on product ID

**Styling Architecture:**
- Tailwind utility classes for component styling
- Custom theme configuration with brand colors
- Responsive breakpoints: xs(450px), sm(575px), md(768px), lg(992px), xl(1200px), 2xl(1400px)  
- Dark mode support via class-based theme switching

## Notes

This is a business website template adapted for CV Anicon, a precast concrete company founded in 2018. The codebase is based on a Next.js startup template with customizations for the concrete products industry. Some template features like Testimonials, Pricing, and Contact forms remain in the code but are commented out in the home page. The product detail system has an interesting limitation where products 6-8 only have image carousels without detailed specification components, suggesting incomplete product documentation for those items.




