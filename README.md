Nexus

Shivam Jha — 5th Semester B.Sc. Student at Pandit Deendayal Energy University (PDEU)

Nexus is a Python-based project I built to make learning from textbooks easier and more meaningful.

The idea behind Nexus is simple: instead of treating a textbook PDF as just a collection of pages, Nexus breaks it down, finds important parts of the book, gives those parts a meaningful representation, and then finds the sections that are most relevant to a question.

The current version focuses on the core system rather than the final website or AI-generated answer.

How Nexus Works

Nexus currently works in four phases:

PDF → Text Extraction → Content Mapping → Embeddings → Semantic Retrieval

Phase 1 — PDF Ingestion and Text Extraction

Phase 1 takes a textbook in PDF format.

First, Nexus checks whether the PDF contains readable text. It then looks at the table of contents and tries to find where the actual textbook content begins while skipping things like the cover, copyright pages, preface, contents, and other front matter.

After that, it extracts the textbook page by page and keeps the page number along with the extracted text.

For my current textbook, the PDF contains 623 pages. Nexus started the extraction from page 20 and extracted 604 pages of textbook content.

The extracted information is saved as:

extracted_text1.json

This file is then used in Phase 2.

Phase 2 — Finding and Organising Important Content

Having all the text is not enough. A textbook contains chapters, sections, equations, figures, and explanations.

Phase 2 goes through the extracted text and tries to identify important parts such as:

Topics and section headings
Equations
Figures
Page numbers
Related content

The purpose is to give the textbook some structure instead of keeping it as one large collection of text.

For my current textbook, Phase 2 identified 445 topics.

The result is saved as:

phase2_topics.json

Phase 3 — Creating Semantic Embeddings

Phase 3 is where Nexus starts working with the meaning of the text.

Nexus uses the all-MiniLM-L6-v2 model through Sentence Transformers. The model is obtained from Hugging Face and is used locally to convert each textbook section into a numerical representation called an embedding.

Each embedding contains 384 numbers.

These numbers allow Nexus to compare the meaning of different pieces of text.

The 445 topics identified in Phase 2 are converted into embeddings and stored in ChromaDB.

The current Phase 3 result is:

445 textbook records
384-dimensional embeddings
ChromaDB used for storage
Phase 4 — Semantic Retrieval

Phase 4 allows me to ask questions about the textbook.

For example:

What is the relation between dielectric and polarization?

Nexus converts the question into an embedding using the same Sentence Transformer model.

It then compares the question with the embeddings stored in ChromaDB and finds the most relevant textbook sections.

For example, Nexus can return:

Result 1 — Susceptibility — Page 204

Result 2 — Polarization — Page 191

Result 3 — Gauss's Law in the Presence of Dielectrics — Page 200

It also shows the distance between the question and each result, along with the actual textbook content.

This allows me to see what information Nexus found instead of simply receiving an AI-generated answer.

Where the LLM Comes In

The LLM is an optional part of the system.

The idea is that after Nexus retrieves the most relevant sections, those sections can be given to an LLM such as ChatGPT or another model. The LLM can then turn the retrieved textbook content into an easy-to-understand answer.

The complete idea is:

Question → Nexus Retrieval → Relevant Textbook Content → LLM → Explanation

This means the LLM does not have to be responsible for finding the textbook information itself.

Why This Is a Backup Version

The original version of Nexus also had a generative LLM/API layer.

That layer depended on an external model and API. When that model/API became unavailable, the final answer-generation part stopped working.

However, the main Nexus system was still working:

The PDF could still be processed.
The textbook structure could still be identified.
The embeddings could still be generated.
The vectors could still be stored in ChromaDB.
Relevant textbook sections could still be retrieved.

So I created this version as a backup of the working core of Nexus.

The main system is not dependent on one particular LLM or API provider.

The LLM can be replaced later without rebuilding the entire textbook processing and retrieval system.

Why I Am Using the Terminal Instead of the Website

The earlier version of Nexus had a Streamlit interface.

For this backup, I intentionally chose to use the terminal.

The reason is that I want to clearly see what the system is actually doing.

The terminal shows the complete process:

623 PDF pages

604 pages extracted

445 topics identified

445 embeddings created

384 dimensions per embedding

445 records stored in ChromaDB

Then I can ask a question and directly see the top results, their pages, distances, and textbook content.

For me, this is more useful while developing and testing the system because I can see the actual working of each stage.

The interface can be added again later. For this backup, the priority is keeping the core system working and easy to verify.

The Main Idea Behind Nexus

I don't want Nexus to simply be another chatbot that gives an answer from a PDF.

I want it to help a student find the right part of a textbook and understand what they need to learn.

The idea is:

Textbook

↓

Understand its structure

↓

Find important concepts

↓

Represent the content semantically

↓

Retrieve relevant information

↓

Use an LLM if an explanation is needed

The goal is not only to give an answer, but to make it easier for a student to reach the right knowledge inside a large textbook.

Current Status
Phase 1 — PDF reading and text extraction ✓
Phase 2 — Topic and content mapping ✓
Phase 3 — Sentence embeddings and ChromaDB ✓
Phase 4 — Semantic textbook retrieval ✓
LLM answer generation — Optional
Streamlit interface — Not included in this backup

This repository is a working backup of the core Nexus system, focused on understanding, organising, and retrieving knowledge from textbooks.
