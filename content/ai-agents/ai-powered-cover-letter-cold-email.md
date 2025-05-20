---
author : "sid"
title : "Building an AI-Powered Cover Letter & Cold Email"
# date : "2020-09-04"
# description : "In this part will take care of your HTML files"
summary: A deep dive into building an intelligent application that helps job seekers create personalized cover letters and cold emails using AI 
tags: ["AI", "NLP", "Web Development", "Career Tools", "OpenAI", "Next.js", "FastAPI", "TypeScript", "AI Agents"]
categories : [
    "AWS",
    "SQS",
]
weight: 100
# series : ["Flask Learing Guide"]
# aliases : ["flask-learing-guide"]
cover:
  image: /images/ai-powered-cover-letter-cold-email.png
  hiddenInList: true
---
[Demo](https://x.com/siddythings/status/1924506017632813218)

In today's competitive job market, standing out with personalized and compelling cover letters and cold emails is crucial. However, crafting these documents can be time-consuming and challenging. This led me to develop an AI-powered application that streamlines this process while maintaining a personal touch.

## Agent Overview
The AI-Powered Cover Letter & Cold Email Generator is a modern web application that leverages artificial intelligence to help job seekers create tailored content for their job applications. The system analyzes both the job description and the candidate's resume to generate contextually relevant and personalized content.

## Technical Architecture

### Frontend Stack
- **Next.js 14**: React framework with App Router for modern web development
- **TypeScript**: For type-safe code and better developer experience
- **Tailwind CSS**: For modern, utility-first styling
- **Framer Motion**: For smooth animations and transitions
- **Shadcn/ui**: For beautiful, accessible UI components
- **React Context**: For efficient state management

### Backend Stack
- **FastAPI**: High-performance Python web framework
- **OpenAI API**: For AI-powered content generation
- **Firecrawl API**: For job description crawl
- **Python-docx**: For DOCX file processing
- **PyPDF2**: For PDF file processing
- **Requests**: For web scraping job descriptions

## Project Structure

```
custom-cover-letter/
├── cover-letter-next/     # Frontend application
│   ├── app/              # Next.js app directory
│   ├── components/       # React components
│   ├── context/         # React context
│   └── public/          # Static assets
│
└── fastapi-backend/      # Backend application
    ├── main.py          # FastAPI application
    └── utils.py         # Utility functions
```

## Implementation Details

### 1. Job Description Extraction
```python
def extract_job_description(url):
    """Extract job description from URL using Firecrawl."""
    try:
        print(os.getenv('FIRECRAWL_API_KEY'))
        app = FirecrawlApp(
            api_key=os.getenv('FIRECRAWL_API_KEY')
        )
        result = app.scrape_url(
            url
        )
        # Firecrawl returns a dict with 'text' and possibly 'html' keys
        print(result)
        text = result.get('markdown', '')
        if not text:
            raise Exception('No text content found from Firecrawl.')
        return text
    except Exception as e:
        raise Exception(f"Error extracting job description with Firecrawl: {str(e)}")

```

### 2. Resume Analysis
The application processes both PDF and DOCX formats:

```python
async def analyze_resume(file: UploadFile):
    file_content = await file.read()
    if file.filename.endswith('.pdf'):
        text = extract_text_from_pdf(file_content)
    else:  # DOCX
        text = extract_text_from_docx(file_content)
    
    analysis = await openai.analyze({
        "model": "gpt-4",
        "messages": [{
            "role": "system",
            "content": "Extract key information from this resume: skills, experience, education, and achievements."
        },
        {
            "role": "user",
            "content": text
        }]
    })
    return analysis
```

### 3. Content Generation
The system generates both cover letters and cold emails:

```python
def generate_cover_letter(resume_text, job_description):
    """Generate a cover letter using OpenAI API."""
    try:
        prompt = f"""Based on the following resume and job description, generate a professional cover letter.
        Focus on matching the candidate's experience with the job requirements.
        
        Resume:
        {resume_text}
        
        Job Description:
        {job_description}
        
        Generate a compelling cover letter that highlights relevant experience and skills."""
        
        response = openai.chat.completions.create(
            model="gpt-4-turbo-preview",
            messages=[
                {"role": "system", "content": "You are a professional career coach helping to write compelling cover letters."},
                {"role": "user", "content": prompt}
            ],
            temperature=0.7,
            max_tokens=1000
        )
        
        return response.choices[0].message.content
    except Exception as e:
        raise Exception(f"Error generating cover letter: {str(e)}")

def generate_cold_email(resume_text, job_description):
    """Generate a cold email using OpenAI API."""
    try:
        prompt = f"""Based on the following resume and job description, generate a professional cold email.
        Make it concise, engaging, and focused on creating a connection.
        
        Resume:
        {resume_text}
        
        Job Description:
        {job_description}
        
        Generate a compelling cold email that demonstrates value and interest."""
        
        response = openai.chat.completions.create(
            model="gpt-4-turbo-preview",
            messages=[
                {"role": "system", "content": "You are a professional career coach helping to write effective cold emails."},
                {"role": "user", "content": prompt}
            ],
            temperature=0.7,
            max_tokens=800
        )
        
        return response.choices[0].message.content
    except Exception as e:
        raise Exception(f"Error generating cold email: {str(e)}") 
```

## API Endpoints

- `POST /api/extract-job`: Extract job description from URL
- `POST /api/generate-cover-letter`: Generate cover letter
- `POST /api/generate-cold-email`: Generate cold email

## Getting Started

### Prerequisites
- Node.js 18+ and npm
- Python 3.8+
- OpenAI API key

### Installation
1. Clone the repository
2. Set up the frontend (Next.js) and backend (FastAPI) environments
3. Configure environment variables for both frontend and backend
4. Start both servers for local development

## Performance & Security

1. **Performance Optimizations**
   - FastAPI's async capabilities for efficient request handling
   - Next.js server-side rendering for optimal performance
   - Efficient file processing with Python-docx and PyPDF2

2. **Security Measures**
   - Environment variable management for API keys
   - Input validation and sanitization
   - Secure file upload handling
   - Rate limiting for API endpoints

## Resources

- [GitHub Repository](https://github.com/siddythings/ai-agents/tree/main/ai-agent-cover-letter)
- [Project Documentation](https://github.com/siddythings/ai-agents/tree/main/ai-agent-cover-letter#readme)

---

*Note: This project is part of the [AI Agents Collection](https://github.com/siddythings/ai-agents). Feel free to contribute or provide feedback through the GitHub repository.* 