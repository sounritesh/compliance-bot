## Problem Statement

The task is to build an API that does the following:

- Take a webpage as the input and it has to check the content in the page against a compliance policy
- Return the findings (non-compliant results) in the response

As an example, we take

Stripe's public compliance policy: https://stripe.com/docs/treasury/marketing-treasury
Lets test it against https://www.joinguava.com/

The task is to build a simple API in any language that checks the webpage copy against the policy and report the findings.
You can use OpenAI or any open-source LLMs for this purpose

## Quick Start (for Linux and Mac)

```
# clone the repository:
git clone https://github.com/sounritesh/compliance-bot.git

# change directory to the repository root
cd compliance-bot

# create a .env file
touch .env

# paste your open ai api key as OPENAI_API_KEY
```

### Setup and Installation

#### Setup using virtualenv

```
# create a virtual environment, make sure you have Python>=3.9.2
python3 -m virtualenv botenv

# activate the virtual environment
source env/bin/activate

# install and setup requirements
pip install -e .
```

#### Setup using Docker

```
# build docker image
# for mac
docker build --platform=linux/arm64 -t compliance-bot .

# for linux
docker build -t compliance-bot .

# run the docker image as a container
docker run -it --rm -p 80:80 -t compliance-bot
```

### Usage

```
# make a post call to /inference/ endpoint with a body containing the webpage url
# { "url": "page_url" }
curl -H 'Content-Type: application/json' \
      -d '{ "url": "https://www.joinguava.com/"}' \
      -X POST http://0.0.0.0:80/inference/

# sample response
{
    "message": "OK",
    "method": "POST",
    "status-code": 200,
    "timestamp": "2024-03-01T13:24:45.457027",
    "url": "http://0.0.0.0:80/inference/",
    "data": {
        "compliance_issues": [
            {
                "content": "Get The Banking You Deserve",
                "content_source": "https://www.joinguava.com/",
                "reason": "Uses the term 'banking' which is prohibited for non-bank financial services."
            },
            {
                "content": "digital banking and networking platform",
                "content_source": "https://www.joinguava.com/",
                "reason": "Uses the term 'digital banking' which is avoided for non-bank financial institutions."
            },
            {
                "content": "Create your business checking account",
                "content_source": "https://www.joinguava.com/",
                "reason": "Uses the term 'business checking account' which is a term to avoid as Guava is not a bank."
            },
            {
                "content": "Easily connect your business checking account to Venmo, Etsy, Shopify, Stripe",
                "content_source": "https://www.joinguava.com/",
                "reason": "Mentions 'business checking account' connection which is a term to avoid."
            },
            {
                "content": "We're not just a banking platform.",
                "content_source": "https://www.joinguava.com/",
                "reason": "Uses the term 'banking platform' which is not permitted for non-bank entities."
            },
            {
                "content": "Guava isn’t just a banking platform, it’s a community.",
                "content_source": "https://www.joinguava.com/",
                "reason": "Terms 'banking platform' used which should be avoided to prevent implying it is a bank."
            }
        ]
    }
}
```
