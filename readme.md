## How the data was collected

I targeted the following 5 universities: 
- NYU 
- Arizona State University
- University of Michigan
- University of Texas at Austin
- Stanford University

For each university, I searched the following queries on Google: 
```
site:[uni].edu "generative AI" AND ("enterprise license" OR "access")
```
```
"[University Name]" AND ("OpenAI" OR "ChatGPT Enterprise" OR "Microsoft" OR "AI" OR "Gemini" OR "Anthropic" OR "Claude" OR "Meta") AND "partnership"
```
```
site:[uni].edu "custom AI" OR "generative AI platform"

```
```
"[University Name]" AND "partnership" AND "AI" AND "[AI company name]"
```

Then I skimmed through the articles brought up by the search engine. If the article seemed promising, it was fed to Gemini with some specific instructions. 
### Setting Up Gemini
#### The System Prompt for the LLM
##### Role and Objective
Your role is to identify and extract data regarding formal partnerships between Higher Education Institutions and AI Industry Leaders (e.g., OpenAI, Google, Anthropic, Microsoft, Meta). 
You are building a structured dataset from provided URLs. Your goal is to capture institutional-level collaborations where the university is receiving specific AI resources, tools, or research support.

##### The Extraction Template
You must output all applicable partnerships using this exact JSON structure:
```
{
  "university": "Institution Name",
  "source_link": "URL",
  "ai_company": "Partner Name",
  "launch_date": "Date",
  "target_audience": [
    "Students",
    "Faculty",
    "Staff"
  ],
  "resources_provided": [
    {
      "name": "Resource Name",
      "description": "Resource description."
    }
  ]
}
```
##### Mandatory Filtering Logic
1. The Industry Leader Rule
  Only generate a JSON if the partnership is between a university and a recognized AI Industry Leader.
  If the partnership is with a company that is not a primary AI developer, or if the collaboration is between two corporations (without a university as a primary partner), state that the link is not applicable and do not generate a JSON.

2. The Institutional Value Rule
  Focus on partnerships that provide resources like enterprise software access, API credits, or dedicated research grants.
  If the link describes a general career forum, a recruitment session, or a one-off guest lecture, it is not applicable.

3. The Redundancy Rule
  If the specific partnership and tool set have already been captured in a previous turn for that university, do not repeat the JSON. Simply state that the JSON has already been generated for that specific partnership.

The results returned are compiled into the file: `ai_collaboration.json`
