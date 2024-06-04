# Vertex AI Agent Builder Guide
Welcome to the world of building intelligent conversational agents with Google Cloud Vertex AI Agent Builder! This comprehensive guide empowers you to navigate your agent's configuration with ease, unlocking the full potential of this powerful no-code solution.

As you delve into crafting your AI agent, understanding its configuration is paramount. The configuration acts as the blueprint for your agent's functionality, defining its behavior, responses, and overall capabilities. By mastering this aspect, you'll be able to fine-tune your agent and tailor it to seamlessly address your specific customer needs.

# Create a Google Cloud Storage Bucket that will contain the PDF documents that will be ingested in the Agent KBs.
- Create a bucket with a unique name
- Inside create two folders, one for financial documents and one for technical documents
- Upload you documents in the appropriate folder. You find the documents in this repo in the docs folder

# Open Agent Builder and create a new Agent by clicking on the Create App
- Select Agent currently in PREVIEW. See screenshot below

![plot](/images/001_AgentBuilder_CreateApp.png)

- Provide a name and a global location
- Agree and Create the Agent

# Configure your first Agent
- Provide a unique Name
- Goal: This is the context given to Gemini. Read the documentation to find out how to write an effective Goal [https://cloud.google.com/dialogflow/vertex/docs/concept/goals]
- Specific Instructions: Use well designed, concise instructions to describe the Agent behavior. Read the documentation to find out how to write instructions [https://cloud.google.com/dialogflow/vertex/docs/concept/goals]

- Use placeholders for AGENTS and/or TOOLS at this stage. You can populate them afterwords. 

- Wanna be a Pro? Think about a possible instructions that can prevent your Agent to hallucination behavoiurs. Hint... Out of Context matters.

# Configure Agent Tool and Data store

Tools can be used by Agent to perform one specific operation. In our scenario we want to retrieve information out from a knowledge base given a specific user query. For this reason we will use a Data store tool which can be used by an Agent App for answers to end-user's questions from your data stores (indeed your KB). The data store type can be one of the following:

- PUBLIC_WEB: A data store that contains public web content.
- UNSTRUCTURED: A data store that contains unstructured private data like PDFs
- STRUCTURED: A data store that contains structured data (for example FAQ).

Here is how you create the Agent Tool

![plot](/images/002_AgentBuilder_CreateTool.png)

Once you are in there, do:

- Provide a unique name
- Type equal to Datastore
- Description: Describe the KB content that this Tool will have access to.

Save and on the same page create a Data store. This will contain your KB. Follow the blue link. This will open a new page

![plot](/images/003_AgentBuilder_CreateDatastore.png)

In the new page, provide Company name in the Agent Configurations

![plot](/images/004_AgentBuilder_CreateDatastore.png)

Next, create a Data store by clicking the CONTINUE button. Then select Cloud Storage

![plot](/images/005_AgentBuilder_CreateDatastore.png)

Browse the Cloud Storage buckets and select the folder that contains your docs. Leave the bullet on the Unstructured documents (PDF, HTML, TXT and more).

![plot](/images/006_AgentBuilder_CreateDatastore.png)

- Click CONTINUE and enter the Data store name in the next view. 
- Select “Layout Parser” as Document parsing technique in the drop down menu. 
- Leave the Chunk size limit to default (500 tokens). 
- Click Create.

![plot](/images/007_AgentBuilder_CreateDatastore.png)

Terminate the App creation by selecting the newly created Datastore and clicking the create button.

![plot](/images/008_AgentBuilder_CreateDatastore.png)

Go back to the previous page to complete the Data store tool. 

- Hard refresh the webpage to see new options in the page. 
- Select the newly created Datastore from the drop down menu for Unstructured documents type. 
- Leave Grounding enable and the default setting for the summarization. 
- Save at the end

![plot](/images/009_AgentBuilder_CreateDatastore.png)

A this point you should have created your first Agent Data store tool connected to a Datastore. Let's verify that everything is working.
- Navigate to Agent Builder, Apps, Cymbal Customer Agent, Datastores 
- Select one Data store.
- Verify that you have a green check in the "Last document import" description.  

Note1: The loading process takes some time, such as 10 minutes or more. Wait until you have a green check.

Note2: You can see what Documents have been imported in your KB by looking at the Documents tab. You can import additional data if you want by clicking the "IMPORT DATA" button. 

![plot](/images/010_AgentBuilder_CreateDatastore.png)

# Let the Agent use his powerful Tools

Go back to your Agent and replace the placeholders in the Instructions section with ${TOOL: NAME_OF_YOUR_TOOL}. The UI helps you out by autocompleting the name of the tool.

Test your Agent by selecting it and asking a meaningful question.

![plot](/images/011_AgentBuilder_TestAgentWithTool.png)

Expand the Tool window to see the Query in the Tool Input section and the Answer in the Tool output section

![plot](/images/012_AgentBuilder_TestAgentWithTool.png)


# Congrats!!! You have built a fully working GenAI-powered Agent for your company!!!

![plot](/images/GenAI_AgentBuilder_Phase1.gif)

# Want more? Have you thought about Multi-Agents systems with Specialized Agent?

Using prompt engineering and dedicated tools you could have Specialized Agent that search for specific questions in their specific KBs. You can have an Front Desk Agent that receive any customer questions and route them to the appropriate specialized Agent based on the type of question. 

- Review the System Agent Architecture
- Plan and build what tools each Agent needs
- Create specialized KBs as Data stores
- Improve prompts in order to accomplish Agent task

At this stage you should have the knowledge already to accomplish the previous tasks. But there is one more bit that you need to know in order let the Front Desk Agent be able to route customer questions. You need to add routing conversation examples. This is how to do

In the Front Agent configuration, move to the Examples tab and create a new Example. Do this in sequence:

- Provide the Name of the Example
- In the chat box enter “Hi”. The Agent should return an answer like “Hi, how can I help you?”. If you are not happy with the answer you can generate alternative draft messages by clicking the stars icon at the end left of the message. Then you can provide a hint or ask the Gemini model to be creative. 

![plot](/images/013_AgentBuilder_RoutingExample.png)

- Select one of the preferred drafts or continue with the original.

![plot](/images/014_AgentBuilder_RoutingExample.png)

- Ask a specific question to activate the specialized Agent (f.i. “How do I restore factory setting in my router”). The Agent will reply that it cannot provide technical support. This is because we haven’t shown to him how to route the question correctly. 

![plot](/images/015_AgentBuilder_RoutingExample.png)

- Delete the last Agent response using the Bin icon on the left and add an Action to invoke the appropriate Agent.

![plot](/images/016_AgentBuilder_RoutingExample.png)

- In the Agent Invocation page select the appropriate Agent (Cymbal Technical Support Agent), provide a summary as invocation input  and a summary of the invocation output. Terminate by selecting the Agent State to OK. OK state means that the control returns back to the Parent Agent which is in this case Cymbal Front Desk Agent. See example below.

![plot](/images/017_AgentBuilder_RoutingExample.png)

- Complete the Example adding an Agent Response using the blue “Add action” button. Then enter a user input that terminates the sequence. See example below

![plot](/images/018_AgentBuilder_RoutingExample.png)

- Create the Example by using the appropriate button. The final result should look like this

![plot](/images/019_AgentBuilder_RoutingExample.png)