---
layout: post
title: Preprocessing Unstructured Data for LLM Applications
date: 2024-09-27 11:46:00
description: Learn how to preprocess unstructured data for large language models (LLMs) using techniques like Retrieval Augmented Generation (RAG), metadata extraction, and advanced document analysis methods.
tags: fine-tuning llm unstructured-data RAG
categories: llm prompt-engineering AI
thumbnail: assets/img/preprocessing-unstructured-data.png
related_posts: true
toc:
  sidebar: left
---

## Introduction

In the era of advanced AI applications, **preprocessing unstructured data** is the key to unlocking the full potential of **large language models (LLMs)**. With the increasing demand for applications like **Retrieval Augmented Generation (RAG)** systems, effective handling of data from multiple formats—such as PDFs, PowerPoint presentations, Markdown files, and even images—has become critical. Preprocessing not only enables LLMs to interact with external data sources but also ensures that the information is retrieved and presented in a meaningful way, enhancing accuracy and relevance.

This blog will take us through the essential techniques and strategies for preprocessing unstructured data, including **metadata extraction**, **chunking**, and the use of **document layout models** and **vision transformers**. Whether we’re working on business reports, academic papers, or large datasets, mastering these techniques will allow us to build more efficient, scalable, and responsive LLM applications.

## Retrieval Augmented Generation (RAG)

### What is Retrieval Augmented Generation?

**Retrieval Augmented Generation (RAG)** is a powerful approach that enhances **large language models (LLMs)** by grounding their responses on **external, validated information**. Instead of relying solely on pre-trained data, RAG applications retrieve relevant data from external sources such as emails, PDFs, or PowerPoint slides. This information is then integrated into the prompt passed to the LLM, resulting in more **accurate** and **context-rich** responses.

In a typical RAG pipeline, external data is stored in a **vector database**. When a query is made, relevant data is retrieved, **chunked**, and **embedded** into the prompt. This ensures that the LLM can draw on **current** and **relevant information** instead of solely relying on pre-existing knowledge.

### Preprocessing Output

**Preprocessing unstructured data** is essential for preparing content for RAG pipelines. We break down documents into manageable **document elements** and capture **element data** to better organize and retrieve information.

**Document Elements**

Key document elements extracted during preprocessing are crucial to ensure that the right information is used in the LLM’s responses:

- **Title:** The main heading or topic of a document section.
- **Narrative Text:** The core body content containing detailed explanations or descriptions.
- **List Item:** Structured content organized as bulleted or numbered lists.
- **Table:** Data arranged in rows and columns, often representing quantitative or categorical information.
- **Image:** Visual elements or graphics embedded within the document.

**Element Data**

For each document element, element data provides additional context, helping with **filtering** and **organizing** content. This data includes:

- **Filename:** The name of the source document.
- **Filetype:** The type of document (e.g., PDF, PowerPoint, HTML).
- **Page Number:** The page on which the element resides, especially relevant for multi-page documents like PDFs.
- **Section:** The specific section or chapter where the element is located.

This element data plays an important role in ensuring efficient information retrieval in RAG applications.

### Why is Preprocessing Hard?

Preprocessing for LLMs in RAG pipelines presents several challenges due to the diversity in file formats, structures, and the nuances of interpreting different types of content. Let’s explore the key reasons why preprocessing is complex:

**Content Cues**

Different file formats provide different content cues that help signal the structure of the document. Interpreting these cues across various formats is crucial for accurate data extraction:

- **HTML** files use structured tags (e.g., `<h1>`, `<ul>`) to define elements like titles, lists, or tables.
- **PDFs** rely on **visual cues** such as font size, boldness, and spacing to indicate hierarchy, like headings and subheadings.

**Standardization Need**

Each file format—whether **PDF**, **Markdown**, or **HTML**—presents information differently. To ensure consistent processing, it is vital to **standardize the output** across all formats. Standardization ensures that our application can handle content uniformly, regardless of its source. However, this is a difficult task due to the **variability** in document structures and how they indicate content.

**Extraction Visibility**

At times, important information is hidden within the document in ways that aren’t immediately visible in the raw content. For example, tables in PDFs or lists in PowerPoints might not be accessible as plain text, requiring **specialized extraction techniques**. These hidden elements further complicate preprocessing.

**Metadata Insights**

In addition to raw content extraction, gathering **metadata** such as file type, page number, and section is essential for operations like **filtering** and **hybrid search** in RAG applications. However, extracting and understanding metadata is challenging due to the **different internal representations** of various file formats.

## Normalizing Diverse Document Types

In developing applications powered by **large language models (LLMs)**, normalizing content from various document types is critical for efficient data processing. Whether we’re handling **PDFs**, **PowerPoint presentations**, **Word documents**, or **HTML pages**, normalization ensures that all formats are processed uniformly. This simplifies downstream operations, reduces complexity, and makes the entire pipeline more cost-effective.

### Why Normalize Diverse Document Types?

Documents come in a variety of formats, each with its own structure and characteristics. Managing these diverse formats in an LLM application can be challenging. The goal of **normalization** is to standardize this process, ensuring that all document types are broken down into **common elements** like titles, narrative text, or lists. This enables the LLM to process them uniformly, regardless of the source format.

### Benefits of Normalization

1. **Uniform Processing Across Formats:**
   Once documents are normalized, they can be processed with the same code, whether the source is a PDF, PowerPoint, or HTML file. For example, after normalization, we can filter out headers or footers uniformly, eliminating the need for separate logic for each file type.
2. **Consistent Chunking and Cost Reduction:**
   Normalized documents allow for consistent **chunking** operations across all formats, reducing processing complexity and cost. Content extraction is typically the most resource-intensive part of preprocessing, while chunking is less costly. Standardizing document structure enables more efficient experimentation with different chunking strategies, cutting down on computational overhead.

### Data Serialization

Once content is normalized, the next critical step is **data serialization**, which ensures that the preprocessed content can be reused later. **JSON** is a popular and versatile format for serialization due to several advantages:

1. **Common and Well-understood Structure:**
   JSON is universally recognized and widely supported across platforms and systems.
2. **Standard HTTP Response:**
   JSON is natively supported as a **standard HTTP response format**, particularly useful for model-based workloads where documents like PDFs or images are processed through APIs.
3. **Cross-language Support:**
   JSON works seamlessly across programming languages. For instance, we could preprocess a document in Python, serialize the output to JSON, and read it into a JavaScript application.
4. **Streaming Use Cases:**
   JSON is also ideal for **streaming applications**, such as **JSON-L**, where large sets of documents are processed incrementally.

### Preprocessing HTML Pages

**HTML** is one of the most common document types that need to be processed for LLM applications, especially when scraping web content. HTML is considered **semi-structured**, making it relatively easy to categorize content using **tags**. For example:

- **H1 tags** typically indicate titles or headings.
- **Paragraph tags** (`<p>`) are used to signify narrative text.

However, content categorization isn’t always straightforward. While an **H1** tag reliably indicates a title, **paragraph** tags might include long descriptive text or short, capitalized strings that resemble headings. In such cases, **natural language processing (NLP)** techniques combined with HTML tags can help classify content more accurately.

Tools like the **unstructured open-source library** simplify HTML preprocessing. Using functions like `partition_html()`, we can break down the document into core elements. These elements are then converted into a dictionary and serialized into **JSON format** for reuse. This structured format allows easy navigation of key sections, such as paragraphs or headers.

### Preprocessing PowerPoint Files

In many business contexts, **PowerPoint presentations** are another common document type that LLM applications need to process. Like HTML, PowerPoint files are semi-structured, but the layout and formatting of elements differ significantly.

**Titles**, **narrative text**, and **bullet points** are formatted distinctively in PowerPoint files.

Using the same **normalization principles** as with HTML, PowerPoint presentations can be broken down into core components to allow for **uniform processing** across different document types. This ensures that content extracted from PowerPoint files can be treated consistently, reducing complexity in further processing.

### Normalizing PDFs: A More Complex Challenge

Compared to HTML and PowerPoint, **PDFs** are often more complex because they rely heavily on **visual cues** to convey structure. This requires a different approach for normalization.

In formats like HTML and PowerPoint, predefined structures such as `<h1>` tags or bullet points guide the differentiation of document elements. However, **PDFs lack these predefined tags**, so the document's structure needs to be inferred through visual formatting. For example:

- **Bold and underlined text** likely represents a title.
- **Long blocks of unformatted text** typically indicate narrative content.

These visual cues are essential for dividing content into categories such as titles, narrative text, or lists during preprocessing. Successfully normalizing PDFs requires carefully analyzing these formatting characteristics to ensure that the LLM can process them in the same way it handles other document types.

## Metadata Extraction and Chunking

### What is Metadata?

**Metadata** refers to additional information extracted during document preprocessing. It enriches the raw content and structures it for more efficient retrieval in **Retrieval Augmented Generation (RAG)** systems. Metadata exists at both the **document level** and **element level**:

1. **Document-Level Metadata:** Extracted directly from the document properties (e.g., file name, last modified date).
2. **Element-Level Metadata:** Inferred during preprocessing (e.g., element type like Title, hierarchical structure, page number).

### Semantic Search for LLMs

**Semantic search** is an essential part of a RAG system, allowing it to retrieve documents from a **vector database** based on their similarity to a query. Here's how it works:

1. **Vector Database:**
   Each document is embedded into a **vector space**, where documents with semantically similar content are placed close to each other.
2. **Similarity Score:**
   The system calculates a **similarity score** to determine how close the query vector is to the document vectors. Documents with closer vectors are more similar in content.
3. **Retrieval:**
   The documents with the highest similarity scores are retrieved and fed into the LLM to ensure that the content returned is relevant.

### Hybrid Search

While **semantic search** is effective for many tasks, it has some limitations, especially when additional contextual filters like time relevance or document structure are needed. **Hybrid search** combines **semantic similarity** with **metadata filtering** to refine the results more accurately.

**Challenges with Semantic Search:**

1. **Too Many Matches:**

   Semantic search can return a large number of results, especially when documents cover the same topic. For instance, a search for "Super Bowl" may return both pre-game and post-game content, even if we're only interested in the results.

2. **No Recency Filter:**

   By default, semantic search doesn't prioritize more recent documents. This becomes problematic when we're seeking the latest information.

3. **Loss of Sectional Information:**

   Semantic search doesn't consider document structure, such as titles or sections, which can lead to fragmented or less contextually relevant results.

**Benefits of Hybrid Search:**

1. **Combining Semantic and Structured Data:**

   Hybrid search improves upon semantic search by incorporating **metadata filters** (e.g., date, section). This enables more accurate results, as the system can restrict the search to more specific content.

2. **Example Use Case:**

   For a query like "Super Bowl results," semantic search may return both pre-game predictions and post-game results. With **hybrid search**, we can filter out documents created before the game, ensuring that only post-game results are returned.

### Chunking

**Chunking** involves breaking down large documents into smaller, manageable pieces, or **chunks**. This is necessary for two main reasons:

1. **LLM Context Window Limitations:**

   LLMs have a limited context window, meaning we can only pass a certain amount of text at a time. Chunking ensures that only relevant pieces of the document are sent to the LLM, preventing overload and improving response quality.

2. **Cost Efficiency:**

   Larger context windows are more expensive to process. By chunking the content, we can save on inference costs by sending only the required information to the LLM.

**Chunking from Document Elements**

There are multiple methods for chunking content, including **chunking by characters** or **tokens**. However, the most efficient approach is **element-based chunking**, which uses metadata to create more contextually accurate chunks.

1. **Traditional Chunking:**

   In traditional chunking, documents are split into even-sized chunks based on a threshold (e.g., character count). However, this method can fragment content across chunks. For example, a section discussing "Open Domain Question Answering" might bleed into another chunk about "Abstractive Question Answering," leading to less relevant results.

2. **Element-Based Chunking:**

   Instead of cutting the document arbitrarily, element-based chunking breaks down documents into **atomic elements** (titles, narrative text, lists). Using **logical break conditions**, like creating a new chunk whenever a title is detected, keeps content that belongs together intact. For example, when chunking by title, all content related to "Open Domain Question Answering" will remain in the same chunk, while irrelevant sections are excluded.

This leads to:

- **Better Search Results:** More relevant content is retrieved from the vector database.
- **More Relevant Prompts:** The LLM receives chunks that are contextually coherent.
- **Improved LLM Responses:** Cleaner, more accurate responses to user queries.

## Preprocessing PDFs and Images

When working with unstructured document types like **PDFs** and **images**, which lack internal tags or structured data, advanced preprocessing techniques are required. These documents often require visual cues to infer structure, and this leads to two primary methodologies for **document image analysis**: **Document Layout Models** and **Vision Transformers**. Each approach has its own strengths and limitations when extracting and structuring data from unstructured documents.

### Document Layout Model

A **Document Layout Model** focuses on identifying and labeling the structure of a document by recognizing its components, such as titles, paragraphs, and lists, through **bounding boxes**. These bounding boxes enable the extraction of specific content elements from a document by identifying regions of interest like narrative text, tables, or headings.

**How it Works:**

1. **Bounding Box Identification:**
   The model scans the document and draws bounding boxes around elements like titles, paragraphs, and tables, effectively isolating them.
2. **Text Extraction:**
   After labeling the boxes, text is extracted from within them. If the text is not embedded in the document (e.g., an image-based PDF), **Optical Character Recognition (OCR)** techniques are applied to extract the text. However, in some cases, such as PDFs with embedded text, the bounding box alone can be used to trace and extract content directly from the document.

The **YOLOx model**, an object detection model, is a popular choice for document layout detection. This architecture excels in detecting and labeling document elements by drawing bounding boxes around them.

### Vision Transformers

Unlike Document Layout Models, **Vision Transformers** are designed to extract text and structured information from PDFs and images in a single step. These models bypass the need for bounding boxes and OCR, accepting an image as input and producing **text** or even **structured JSON** as output directly.

### How it Works:

1. **Input:** The **Vision Transformer** takes a document image as its input.
2. **Processing:** It processes the document and generates a valid output, often in a **structured format** like JSON.
3. **Output:** The output JSON contains both the text content and the categories of document elements (e.g., title, narrative text, list item).

One common model architecture for vision transformers is the **Donut Architecture** (Document Understanding Transformer). This model can be trained to output structured data like JSON, making it particularly useful for documents like forms or invoices, where structured data is essential.

### Advantages of Document Layout Models

1. **Trained on Fixed Element Types:**
   Document layout models are highly effective at recognizing specific, fixed elements such as titles, paragraphs, and tables. This specialization allows for high accuracy in identifying these predefined document structures.
2. **Bounding Box Information:**
   The bounding boxes provide clear, traceable locations of elements within a document. This is useful when the text can be extracted directly from the document without relying on OCR.

### Disadvantages of Document Layout Models

1. **Two Model Calls:**
   Document layout models typically require two separate steps: first, the object detection model identifies and labels document elements, and second, an OCR model is used to extract the text if it's not embedded in the document.
2. **Limited Flexibility:**
   These models are built to work with a **fixed set of element types**. Adapting the model to identify new elements or nonstandard document structures often requires additional retraining, making them less flexible.

### Advantages of Vision Transformers

1. **Flexibility for Nonstandard Document Types:**
   Vision transformers excel in handling nonstandard or complex document types like forms, where key-value pairs or dynamic layouts may be present. Their flexibility allows them to process various document formats with ease.
2. **Adaptable to New Ontologies:**
   Vision transformers can easily adapt to new document structures or element types, allowing for more dynamic usage. Adding new element types is relatively straightforward through **prompting** without requiring model retraining.

### Disadvantages of Vision Transformers

1. **Potential for Hallucination:**
   Vision transformers are generative models, which means they can sometimes produce content that wasn't present in the original document. This issue, known as **hallucination**, can reduce accuracy and reliability in certain cases.
2. **Higher Computational Cost:**
   Vision transformers are computationally expensive compared to document layout models. They require more computing power and tend to process large documents more slowly, which may be a drawback for resource-constrained environments.

## Conclusion

Preprocessing unstructured data is crucial for large language models (LLMs) to function accurately and efficiently. By leveraging methods such as **metadata extraction**, **hybrid search**, **chunking**, and advanced models like **Document Layout Models** and **Vision Transformers**, we can convert diverse and complex documents into structured formats that LLMs can process effectively.

Whether it’s normalizing different file types or extracting valuable information from PDFs and images, these preprocessing techniques form the foundation for scalable and high-performing LLM applications. Equipped with these methods, we can build systems that manage vast quantities of unstructured data while maintaining relevance and precision in their outputs.
