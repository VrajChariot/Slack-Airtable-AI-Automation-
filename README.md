# 🔗 Slack-Airtable-AI Automation

## 🚀 About the Project

This project automates the process of capturing content from specific Slack channels, intelligently enriching it with AI, storing it in a structured database (Airtable), and then automatically generating and sharing relevant LinkedIn post content back into Slack. It's designed to streamline content curation and social media outreach for teams, ensuring valuable links and ideas shared in Slack are transformed into actionable content.

### ✨ Motivation

In collaborative environments, valuable links and content ideas often get lost in Slack threads. This automation acts as a "content catalyst," ensuring that whenever a team member shares a relevant link or content heading in a designated Slack channel, it's captured, processed, transformed into a ready-to-use LinkedIn post, and returned for easy review and sharing. This frees up manual effort and ensures no great idea goes unutilized.

## 🎯 Features

* **Slack Channel Monitoring:** Automatically detects new messages containing links or specific content headings in a designated Slack channel.
* **Content Extraction:** Intelligently extracts the link URL and/or the content heading for processing.
* **Airtable Integration:** Stores the extracted content (link, heading, original Slack message ID) securely in an Airtable database.
* **AI-Powered Content Generation:** Utilizes an AI (Large Language Model) via API to generate engaging and relevant LinkedIn post content based on the captured information.
* **LinkedIn Post Storage:** Saves the AI-generated LinkedIn post draft directly into Airtable for review and easy access.
* **Automated Slack Reply:** Posts the AI-generated LinkedIn content back as a reply in the original Slack thread, providing immediate feedback to the team.
* **Workflow Orchestration:** Leverages a no-code automation platform (Make.com) for seamless integration and conditional logic.

## ⚙️ Technologies Used

* [**Make.com (formerly Integromat)**](https://www.make.com/): Core automation and workflow orchestration platform.
* [**Slack API**](https://api.slack.com/): For monitoring messages and posting replies.
* [**Airtable API**](https://airtable.com/api): For database management and structured data storage.
* **Large Language Model (LLM) API:** Used for AI-driven text generation (e.g., Gemini 2.0 Flash via Google Generative Language API).
* **JSON:** For data structuring and transfer between modules.

## 🏗️ Architecture Overview

The automation operates as a sequential workflow orchestrated by Make.com:

1.  **Trigger:** A new message in a specified Slack channel.
2.  **Parsing & Filtering:** The message content is analyzed to identify links or content headings.
3.  **Airtable Record Creation:** Extracted data is sent to Airtable, creating a new record.
4.  **AI Transformation:** The captured content is passed to an LLM API with a carefully crafted prompt to generate a LinkedIn post draft.
5.  **Airtable Update:** The generated LinkedIn post is updated in the corresponding Airtable record.
6.  **Slack Notification:** The final LinkedIn post draft is sent back to the original Slack channel as a reply.

## 🚀 Setup and Installation

To set up and run this automation, you will need accounts for Make.com, Slack, Airtable, and access to an LLM API (e.g., Google's Gemini API).

1.  **Airtable Setup:**
    * Create a new Airtable Base.
    * Create a Table (e.g., "Content Ideas") with the following fields:
        * `Heading` (Single line text)
        * `Link` (URL)
        * `LinkedIn Post Draft` (Long text)
        * `Slack Message ID` (Single line text - used for replying)
        * `Status` (Single select: `New`, `Processed`, `Shared`)

2.  **Make.com Scenario Setup:**
    * Create a new scenario in Make.com.
    * **Module 1: Slack - Watch Events:**
        * Choose "New Event" or "Watch Messages."
        * Connect your Slack workspace.
        * Select the specific channel to monitor.
        * Configure filters to only trigger for messages containing links or specific keywords for content headings.
    * **Module 2: Airtable - Create a Record:**
        * Connect your Airtable account.
        * Select your Base and Table.
        * Map the `Heading`, `Link`, and `Slack Message ID` from the Slack module to the Airtable fields. Set `Status` to `New`.
    * **Module 3: HTTP - Make a request (for LLM API):**
        * **URL:** Your LLM API endpoint (e.g., `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=YOUR_API_KEY`)
        * **Method:** `POST`
        * **Headers:** `Content-Type: application/json`
        * **Body (JSON):** Construct the prompt using the Slack message content.
            ```json
            {
                "contents": [
                    {
                        "role": "user",
                        "parts": [
                            { "text": "Draft a concise and engaging LinkedIn post (max 300 words) based on this content/link: [Map Slack message text or extracted content here]" }
                        ]
                    }
                ]
            }
            ```

    * **Module 4: Airtable - Update a Record:**
        * Connect your Airtable account.
        * Select your Base and Table.
        * Use the `Record ID` from Module 2.
        * Map the AI-generated text from Module 3's response to the `LinkedIn Post Draft` field. Set `Status` to `Processed`.
    * **Module 5: Slack - Create a message:**
        * Connect your Slack workspace.
        * Choose "Send a reply to a thread."
        * Map the `Thread ID` (or `Timestamp` to create a thread) from Module 1.
        * Map the `LinkedIn Post Draft` from Module 4 as the message text.

3.  **Activate the Scenario:** Turn on your Make.com scenario.

## 💡 Usage

Simply post a message containing a link or a clear content heading into your designated Slack channel. The automation will then run, process the content, generate a LinkedIn post, and reply in the same thread with the draft.

## 🔮 Future Enhancements

* **Customizable AI Prompts:** Allow users to define custom prompts for different types of content/LinkedIn posts.
* **Multi-Platform Publishing:** Expand to automatically publish or draft posts for other social media platforms (Twitter, Instagram, etc.).
* **Sentiment Analysis:** Integrate sentiment analysis of the original Slack content before generating the post.
* **User Feedback Loop:** Add a mechanism in Slack for users to approve/edit the AI-generated post before it's finalized/shared.
* **Error Notifications:** Set up alerts for failed automation runs.
* **Advanced Analytics:** Track which generated posts are most effective or shared.

## 🚧 Challenges & Learnings

* **Real-time Trigger Reliability:** Ensuring the Slack trigger was consistently reliable and handling various message formats (plain text, links, attachments).
* **API Rate Limits & Async Execution:** Managing API rate limits for both Slack and the LLM, and designing the workflow to handle asynchronous operations efficiently without breaking.
* **Effective AI Prompt Engineering:** Crafting prompts that consistently yield high-quality, concise, and engaging LinkedIn content from diverse input. This required iterative testing and refinement.
* **Robust Data Mapping & Transformation:** Accurately mapping data fields across Slack, Make.com, Airtable, and the LLM API, and handling necessary data transformations (e.g., text formatting, URL extraction).
* **Conditional Logic for Workflow Branching:** Implementing logic to handle different scenarios (e.g., messages with only text vs. messages with links, presence of specific keywords).

## 🎥 Live Demo

A live demo video/GIF demonstrating the full workflow from Slack input to Slack reply is available upon request.

## 📞 Contact

Vraj Shah - [vraj0410shah@gmail.com](mailto:vraj0410shah@gmail.com)

[GitHub Profile](https://github.com/vrajchariot) | [LinkedIn Profile](https://linkedin.com/in/vrajshah04/)
