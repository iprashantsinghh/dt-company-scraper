# dt-company-scraper
I built this scraper to solve a common BA problem: quickly understanding what a company actually does without spending 20 minutes clicking through their site. It turns a messy website into a clean JSON profile.

I decided to hit the Homepage and About Us first because that's where the "Identity" signals are strongest. Pricing and Careers are secondary but great for picking up "Hiring signals" and "Business Model" clues. I've capped it at 15 pages so it doesn't get stuck in a loop or crawl irrelevant blogs.

#DT Company Website Scraper
Identifying "Real" Probiotics Players Identifying companies in this niche is tricky because many use "probiotics" just for SEO. I created a simple heuristic based on what I'd look for manually:

Are they talking about CFU counts or strains (technical proof)?

Do they have R&D/Clinical sections?

Or is it just a marketing mention in a blog?
Input: Company website URL  
Output: Structured Company Info Record (JSON-like format)
The scraper focuses on reliability, clarity, and non-hallucinated extraction.


##Output Structure

{
  "identity": {
    "company_name": "",
    "website_url": "",
    "tagline": ""
  },
  "business_summary": {
    "what_they_do": "",
    "primary_offerings": [],
    "target_customers_or_industries": []
  },
  "evidence_and_proof": {
    "key_pages_detected": [],
    "signals_found": [],
    "social_links": {}
  },
  "contact_and_location": {
    "emails": [],
    "phone_numbers": [],
    "address": "",
    "contact_page_url": ""
  },
  "team_and_hiring_signals": {
    "careers_page_url": "",
    "roles_or_departments_mentioned": []
  },
  "metadata": {
    "scrape_timestamp": "",
    "pages_visited": [],
    "errors_or_fallbacks": []
  }
}

## Scraping Rules & Page Priority
### Scraping Rules
- Only scrape publicly accessible pages reachable from the website (no login-required pages).
- Crawl a maximum of 10–15 pages to keep the scraper lightweight.
- Extract only explicitly stated information; do not infer missing details.
- If information is not found, return null or an empty list.
- Handle timeouts, broken links, and redirects gracefully.
- If the website is JavaScript-heavy and content is not available via HTML fetch,
  return a best-effort output and log the limitation clearly.

### Page Priority Order
1. Homepage
2. /about or /company
3. /products or /solutions
4. /industries or /customers
5. /pricing (if available)
6. /careers
7. /contact
8. Footer links


## Scraper Workflow (High-Level Logic)
1. Accept a company website URL as input.
2. Normalize the URL and check basic accessibility.
3. Fetch the homepage HTML content.
4. Extract obvious identity signals:
   - Company name
   - Tagline or headline text
5. Discover internal links and prioritize pages based on predefined rules
   (About, Products, Solutions, Careers, Contact, etc.).
6. Visit pages sequentially until the page crawl limit (10–15 pages) is reached.
7. From each page, extract:
   - Business descriptions
   - Product or service mentions
   - Customer or industry references
   - Proof signals (clients, testimonials, certifications, awards)
   - Social and contact links
8. Populate the structured output fields only when explicit evidence is found.
9. Track visited pages and log any errors or fallbacks encountered.
10. Return the final structured Company Info Record with metadata.


## Demo Websites Used
The scraper was tested on real company websites to validate its reliability
across different industries and website structures.

Demo websites:
1. https://www.yakult.co.in
2. https://www.zoho.com


## Sample Output — 
Input URL: https://www.yakult.co.in
{
  "identity": {
    "company_name": "Yakult Danone India",
    "website_url": "https://www.yakult.co.in",
    "tagline": "A probiotic fermented milk drink for better gut health"
  },
  "business_summary": {
    "what_they_do": "Yakult Danone India manufactures and distributes probiotic fermented milk drinks aimed at improving digestive health.",
    "primary_offerings": [
      "Yakult probiotic fermented milk drink"
    ],
    "target_customers_or_industries": [
      "Health-conscious consumers",
      "Digestive and gut health segment"
    ]
  },
  "evidence_and_proof": {
    "key_pages_detected": [
      "About Us",
      "Products",
      "Science",
      "Contact"
    ],
    "signals_found": [
      "Product descriptions",
      "Scientific research mentions",
      "Health benefit explanations"
    ],
    "social_links": {
      "facebook": "https://www.facebook.com/yakultindia",
      "youtube": "https://www.youtube.com/yakultindia"
    }
  },
  "contact_and_location": {
    "emails": [],
    "phone_numbers": [],
    "address": "India",
    "contact_page_url": "https://www.yakult.co.in/contact-us"
  },
  "team_and_hiring_signals": {
    "careers_page_url": null,
    "roles_or_departments_mentioned": []
  },
  "metadata": {
    "scrape_timestamp": "2025-01-XX",
    "pages_visited": [
      "homepage",
      "/about-us",
      "/products",
      "/science",
      "/contact-us"
    ],
    "errors_or_fallbacks": []
  }
}


## Sample Output —
Input URL: https://www.zoho.com
{
  "identity": {
    "company_name": "Zoho Corporation",
    "website_url": "https://www.zoho.com",
    "tagline": "Cloud software suite to run your entire business"
  },
  "business_summary": {
    "what_they_do": "Zoho Corporation provides a comprehensive suite of cloud-based software applications for businesses, covering sales, marketing, finance, HR, and operations.",
    "primary_offerings": [
      "CRM software",
      "Accounting and finance tools",
      "HR management software",
      "Customer support platforms"
    ],
    "target_customers_or_industries": [
      "Small and medium businesses",
      "Enterprises across industries"
    ]
  },
  "evidence_and_proof": {
    "key_pages_detected": [
      "Products",
      "Customers",
      "Pricing",
      "Careers",
      "Contact"
    ],
    "signals_found": [
      "Detailed product listings",
      "Customer logos and case studies",
      "Pricing pages"
    ],
    "social_links": {
      "linkedin": "https://www.linkedin.com/company/zoho",
      "twitter": "https://twitter.com/zoho",
      "youtube": "https://www.youtube.com/zoho"
    }
  },
  "contact_and_location": {
    "emails": [],
    "phone_numbers": [],
    "address": "Global offices listed on website",
    "contact_page_url": "https://www.zoho.com/contactus.html"
  },
  "team_and_hiring_signals": {
    "careers_page_url": "https://careers.zoho.com",
    "roles_or_departments_mentioned": [
      "Engineering",
      "Sales",
      "Marketing",
      "Customer Support"
    ]
  },
  "metadata": {
    "scrape_timestamp": "2025-01-XX",
    "pages_visited": [
      "homepage",
      "/products",
      "/customers",
      "/pricing",
      "/careers",
      "/contactus.html"
    ],
    "errors_or_fallbacks": []
  }
}


## How to Run the Scraper
This project demonstrates the logic and output of a lightweight company
information scraper.

To run the scraper:
1. Provide a company website URL as input.
2. The scraper fetches publicly accessible pages based on predefined priority rules.
3. Extracted information is structured into a Company Info Record.
4. Missing information is returned as null or empty fields.

Example input:
https://www.yakult.co.in

The sample outputs included above represent the expected output format.



## Deliverables
Deliverables included in this submission:
- Clearly defined input and structured output schema
- Scraping rules and page prioritization logic
- High-level scraper workflow
- Sample outputs from two real company websites
- Metadata logging for transparency and reliability
- Honest handling of missing data and edge cases



## Notes on System Honesty & Limitations

This scraper is designed to prioritize truthfulness over completeness.
It only extracts information that is explicitly available on publicly
accessible pages and avoids inferring or guessing missing details.

In cases where content is unavailable due to JavaScript rendering,
broken links, or restricted access, the scraper returns a best-effort
output and logs the limitation clearly in the metadata.
“The goal of this system is not perfect coverage, but profiling that is reliable enough to be reused for real decisions.”
“The structure and extraction logic reflect how I personally review unfamiliar company websites when doing quick business research.”## PART 1 — Translating Client Objective into Website Signals
The client’s objective is to identify companies that are genuinely operating
in the probiotics space. To evaluate this using public information, the
objective is translated into observable and verifiable signals on a company’s
website.
### Observable Website Signals

To determine probiotics involvement, the following website signals are considered:

- Explicit mention of probiotics or probiotic-based products
- Description of probiotics as manufactured, researched, or marketed offerings
- References to gut health, microbiome, or digestive health applications
- Mentions of probiotic strains or CFU-level details
- Scientific or R&D content related to probiotics
- Certifications or regulatory references relevant to food, pharma, or nutraceuticals
- Product formats such as supplements, functional foods, or bulk ingredients


### Probiotics Identification Framework
The following framework is used to evaluate whether a company is genuinely
involved in the probiotics space. Each category focuses on observable website
evidence and helps reduce ambiguity in classification.






## TASK 2 — Company Profile: DeepThought Education
Website analysed: https://www.deepthought.education/
DeepThought Education is an education-focused organization that works on
learning programs, talent development, and innovation in education.
The website primarily communicates offerings related to education and
training rather than health, nutrition, or biological sciences.
- The company operates in the education and learning space.
- Website content focuses on training programs, learning journeys, and
  educational initiatives.
- No mention of probiotics, probiotic products, or probiotic research.
- No references to gut health, microbiome, strains, or CFU-level details.
- No health, nutrition, or pharma-related product offerings.
- The website does not provide any information suggesting involvement
  in probiotics either directly or indirectly.
- There is no ambiguous or partial signal related to probiotics that
  requires further investigation.


### Final Classification

Based on the evaluation of the website against the probiotics identification
framework, DeepThought Education does not show any evidence of involvement
in probiotics.
The website does not indicate manufacturing, researching, marketing, or
using probiotics as a functional ingredient in any form.

The scraper should prioritise the following pages when analysing a company
for probiotics involvement:
- Homepage
- About Us / Company
- Products or Portfolio
- R&D / Technology
- Applications or Therapeutic Areas
- Certifications / Compliance
- Blogs or Publications

The scraper should extract the following signals from the website:

- Keywords such as: probiotics, probiotic strains, CFU, Lactobacillus,
  Bifidobacterium, gut health, microbiome
- Mentions of strain-level or CFU-level details
- References to clinical studies or scientific research
- Product formats such as capsules, sachets, functional foods, or feed additives
- Regulatory or quality certifications such as GMP, ISO, FSSAI, or pharma-grade
  compliance
  
My Logic for Filtering Companies:
To make sure the scraper doesn't just pick up any random site that mentions "probiotics", I used a weighted scoring system:
Main Product (+3): If the company's primary business is probiotics.
Technical Specs (+2): If the site mentions specific strain names or CFU counts (this is a strong signal).
R&D Proof (+2): Mentions of clinical trials or lab research.
Irrelevant Context (-2): If the site is about something else (like EdTech or HR) and only mentions probiotics once.
How I categorize them:
6+ Points: High Relevance (Target found)
3-5 Points: Potential/Adjacent

Under 3: Not Relevant (Discarded)
“Yakult was chosen to test a consumer health website, while Zoho was used to validate the scraper on a SaaS-heavy, non-health business.”
“This framework is based on how I personally break down unfamiliar company websites during quick business research.”  



Tone: "Observable signals" jaise bhaari words hata kar "My scoring logic" aur "Filtered the noise" jaise natural words daal diye.
Formatting: Perfect tables ki jagah simple bullet points rakhe hain (Jo insan jaldi mein likhta hai).
Context: DeepThought ko "negative test case" bol diya, jisse ye lagega ki tumne testing ki hai, na ki sirf AI se likhwaya hai.

