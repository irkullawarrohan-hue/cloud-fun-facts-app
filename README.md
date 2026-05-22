Project Overview:

The Cloud Fun Facts Generator is a serverless cloud-native application built to demonstrate how multiple AWS services work together in a real-world workflow. Instead of learning AWS services individually through isolated examples, this project focuses on integrating them into a complete application architecture.

The application allows users to generate random cloud computing facts through a web interface while leveraging a fully serverless backend. Behind the scenes, the project combines AWS Lambda, Amazon API Gateway, DynamoDB, Amazon Bedrock, and AWS Amplify to create a scalable and interactive cloud solution.

The frontend is hosted using AWS Amplify, which communicates with a REST API exposed through Amazon API Gateway. API Gateway triggers an AWS Lambda function that processes incoming requests, retrieves cloud facts from DynamoDB, and enhances responses using Amazon Bedrock for AI-generated output.

This project provided practical experience in designing and deploying serverless architectures, integrating cloud services, managing APIs, handling NoSQL databases, and working with generative AI services within AWS.

The primary goal of this project was to move beyond basic tutorials and build a portfolio-ready application that demonstrates practical cloud engineering skills and real AWS service integration.


Services Used:

AWS Lambda: Serverless backend to generate cloud fun facts.
Amazon API Gateway: Exposes the Lambda as a REST API endpoint.
Amazon DynamoDB: Database for storing facts.
Amazon Bedrock: Generative AI to make facts witty.
AWS Amplify: Hosting service for the React frontend.
AWS IAM: Identity and Access Management for secure permissions.


Architectural Diagram:

<img width="1147" height="621" alt="ba68b9bbca714ba386bf118d7078c8b4" src="https://github.com/user-attachments/assets/e12b44e2-c3bc-4c68-82f3-2261a288840c" />


Created a Lambda Function:

<img width="1716" height="762" alt="0b14908848f249f08caf7bd74c4e37b6" src="https://github.com/user-attachments/assets/e3ac770a-63fe-4a8e-9a82-7c0261bd429c" />
<img width="1892" height="814" alt="d9ff910bd3d74e35b21272cc0146c8e2" src="https://github.com/user-attachments/assets/1ec18d2d-490e-427a-91bf-c50edecf7acf" />


Added lambda code:

    import random
    import json

    def lambda_handler(event, context):
    facts = [
        "AWS S3 was launched in 2006 and still rules cloud storage.",
        "Cloud computing can save companies up to 30% on IT costs.",
        "EC2 was one of the first AWS services to change IT forever.",
        "AWS offers more than 200 services — that’s more than Starbucks drinks!",
        "Cloud lets you pay-as-you-go, just like your Netflix subscription.",
        "The name 'Amazon Web Services' was first used back in 2002.",
        "AWS data centers are so secure they require palm scanners.",
        "Netflix runs most of its infrastructure on AWS.",
        "Amazon DynamoDB can handle more than 10 trillion requests per day.",
        "AWS Lambda was launched in 2014 and started the serverless trend.",
        "Cloud reduces CO₂ emissions by optimizing energy usage.",
        "AWS regions have multiple Availability Zones for reliability.",
        "Amazon originally created S3 to solve its own scaling issues.",
        "More than 80% of Fortune 500 companies use AWS.",
        "Cloud helps startups scale globally without huge upfront costs.",
        "Amazon’s first region outside the US was launched in Ireland (2007).",
        "AWS provides free tiers so students can build projects affordably.",
        "AWS CloudFront is one of the largest CDNs in the world.",
        "Serverless means you never patch servers — AWS does it for you!",
        "AWS is the market leader in cloud with ~32% share (as of 2025)."
    ]
    
    fact = random.choice(facts)
    return {
        "statusCode": 200,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps({"fact": fact})
    }

<img width="1617" height="780" alt="0a032da4c9ad4303ab6498197e486b33" src="https://github.com/user-attachments/assets/4bc307a3-5b2d-4d1a-8f47-cd81d0e4df3f" />


Created API Gateway:


<img width="1724" height="737" alt="0f2c52cbba834b3daab416099307ef37" src="https://github.com/user-attachments/assets/6e077a4f-5438-4259-9171-927cfd1ca213" />

Created DynamoDB Table:

<img width="1883" height="775" alt="6d4f890001194273af4511cb48989468" src="https://github.com/user-attachments/assets/7c0e333f-ae58-43fc-921e-069307691e2d" />
<img width="1694" height="521" alt="ce8d7255cdf3400aa292ec9c57d3bbce" src="https://github.com/user-attachments/assets/39880a01-8449-4f34-9a98-37199c23eae8" />
<img width="1237" height="743" alt="b2298866f056488fb8c4232ae95c8d31" src="https://github.com/user-attachments/assets/9099ac57-efed-4621-9755-9c38a19cc65d" />


Updated Lambda Role:

<img width="1909" height="595" alt="021c25e7f38b4b7893169cc44b88fd26" src="https://github.com/user-attachments/assets/68d1632a-fff9-419b-9056-00db192b87e8" />

Updated Lambda Code:

    import boto3
    import random
    import json

    # Create DynamoDB client
    dynamodb = boto3.resource("dynamodb")
    table = dynamodb.Table("CloudFacts")

    def lambda_handler(event, context):
    # Scan entire table (not efficient for huge tables, but fine here)
    response = table.scan()
    items = response.get("Items", [])

    if not items:
        return {
            "statusCode": 500,
            "body": json.dumps({"error": "No facts found"})
        }

    # Pick random fact
    fact = random.choice(items)["FactText"]

    return {
        "statusCode": 200,
        "headers": {"Content-Type": "application/json"},
        "body": json.dumps({"fact": fact})
    }

<img width="1818" height="720" alt="f00fbb7f191149bcbe6afe4c4d852578" src="https://github.com/user-attachments/assets/98f041b2-e4d4-4357-8370-ebfac26385ed" />


GenAI Version:
  Request Model Access in Bedrock:

  <img width="908" height="557" alt="download" src="https://github.com/user-attachments/assets/f0a44c34-ab7f-4616-a329-e4b8d2a891c3" />
  

Updated IAM Role:


<img width="1881" height="746" alt="3467235046a446d298e50f18bde5bbd7" src="https://github.com/user-attachments/assets/570d03ea-4eac-44da-82bb-30aaba18e7dc" />
<img width="1881" height="746" alt="3467235046a446d298e50f18bde5bbd7 (1)" src="https://github.com/user-attachments/assets/b601c112-c1fe-4b05-86b9-625e49e4a7b4" />


Again updated Lambda Code:

      import boto3
      import random
      import json


    # DynamoDB connection
    dynamodb = boto3.resource("dynamodb")
    table = dynamodb.Table("CloudFacts")


    # Bedrock client
    bedrock = boto3.client("bedrock-runtime")


    def lambda_handler(event, context):
    # Fetch all facts from DynamoDB
    response = table.scan()
    items = response.get("Items", [])
    if not items:
        return {
            "statusCode": 200,
            "headers": {
                "Content-Type": "application/json",
                "Access-Control-Allow-Origin": "*",
                "Access-Control-Allow-Methods": "GET, OPTIONS",
                "Access-Control-Allow-Headers": "Content-Type"
            },
            "body": json.dumps({"fact": "No facts available in DynamoDB."})
        }


    fact = random.choice(items)["FactText"]


    # Messages for Claude 3.5 Sonnet
    messages = [
        {
            "role": "user",
            "content": f"Take this cloud computing fact and make it fun and engaging in 1-2 sentences maximum. Keep it short and witty: {fact}"
        }
    ]


    body = {
        "anthropic_version": "bedrock-2023-05-31",
        "max_tokens": 100,
        "messages": messages,
        "temperature": 0.7
    }


    try:
        # Call Claude 3.5 Sonnet on Bedrock
        resp = bedrock.invoke_model(
            modelId="anthropic.claude-3-5-sonnet-20240620-v1:0",
            body=json.dumps(body),
            accept="application/json",
            contentType="application/json"
        )


        # Parse response
        result = json.loads(resp["body"].read())
        witty_fact = ""


        # Claude v3 response: look inside "content"
        if "content" in result and result["content"]:
            for block in result["content"]:
                if block.get("type") == "text":
                    witty_fact = block["text"].strip()
                    break


        # Fallback if empty or too long
        if not witty_fact or len(witty_fact) > 300:
            witty_fact = fact


    except Exception as e:
        print(f"Bedrock error: {e}")
        witty_fact = fact


    return {
        "statusCode": 200,
        "headers": {
            "Content-Type": "application/json",
            "Access-Control-Allow-Origin": "*",
            "Access-Control-Allow-Methods": "GET, OPTIONS",
            "Access-Control-Allow-Headers": "Content-Type"
        },
        "body": json.dumps({"fact": witty_fact})
    }



Final Result:

<img width="1280" height="619" alt="27795a6e54624873b0b67eb1d16722ae" src="https://github.com/user-attachments/assets/e30bd3a2-22f7-40b9-a18a-e6ca66fd56f2" />

<img width="1280" height="618" alt="image" src="https://github.com/user-attachments/assets/e1d021d9-feeb-4a63-b213-826a4458757a" />

<img width="1280" height="612" alt="image" src="https://github.com/user-attachments/assets/6d5b6a84-fae7-46f5-94ae-57b0665c1b89" />




