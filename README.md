### EX5 Information Retrieval Using Boolean Model in Python
### DATE : 18.08.2026
### Name : DAKSHINA MOORTHY N D
### Register No. : 212224230049
### AIM: To implement Information Retrieval Using Boolean Model in Python.
### Description:
<div align = "justify">
The Boolean model in Information Retrieval (IR) is a fundamental model used for searching and retrieving information from a collection of documents. It operates on the principles of set theory and logic, where documents are represented as sets of terms or words, and queries are expressed as Boolean expressions using logical operators such as AND, OR, and NOT.
  
### Procedure:
1. ***Initialize the BooleanRetrieval class:*** The BooleanRetrieval class is defined to manage the indexing and searching of documents.
2. ***Constructor and Index Initialization:*** The class constructor initializes an empty index to store the inverted index mapping terms to documents.
3. ***Indexing Documents:***
    <p> a) The index_document method is responsible for indexing documents.
    <p> b) Tokenize the text content of documents, converting them into lowercase terms.
    <p> c) For each term in the document, it adds an entry in the index, associating the term with the document ID. </p>
4. ***Fetch Web Page Text:***
    <p>a) The fetch_webpage_text method uses the requests library to fetch content from a given URL.
    <p>b) Extract text content from the fetched HTML using BeautifulSoup.
    <p>c) The extracted text is returned for further processing.
5. ***Boolean Search:***
    <p>a) The boolean_search method performs Boolean searches on the indexed documents.
    <p>b) Tokenize the input query and iterates through its terms.
    <p>c) For each term in the query, it retrieves documents containing that term and performs Boolean operations (AND, OR, NOT) based on the query's structure.

### Program:
```python
import numpy as np
import pandas as pd


class BooleanRetrieval:

    def __init__(self):
        self.index = {}
        self.documents_matrix = None

    def index_document(self, doc_id, text):
        terms = text.lower().split()
        print("Document -", doc_id, terms)

        for term in terms:
            if term not in self.index:
                self.index[term] = set()

            self.index[term].add(doc_id)

    def create_documents_matrix(self, documents):
        terms = list(self.index.keys())
        num_docs = len(documents)
        num_terms = len(terms)

        self.documents_matrix = np.zeros(
            (num_docs, num_terms),
            dtype=int
        )

        for i, (doc_id, text) in enumerate(documents.items()):
            doc_terms = text.lower().split()

            for term in doc_terms:
                if term in self.index:
                    term_id = terms.index(term)
                    self.documents_matrix[i, term_id] = 1

    def print_documents_matrix_table(self):
        df = pd.DataFrame(
            self.documents_matrix,
            columns=self.index.keys()
        )
        print("\nDocument-Term Matrix:")
        print(df)

    def print_all_terms(self):
        print("\nAll terms in the documents:")
        print(list(self.index.keys()))

    def boolean_search(self, query):
        query = query.lower().strip()

        if " and " in query:
            terms = query.split(" and ")

            if all(term in self.index for term in terms):
                result = self.index[terms[0]].copy()

                for term in terms[1:]:
                    result = result.intersection(
                        self.index[term]
                    )

                return sorted(result)

            return []

        elif " or " in query:
            terms = query.split(" or ")
            result = set()

            for term in terms:
                if term in self.index:
                    result = result.union(
                        self.index[term]
                    )

            return sorted(result)

        elif " not " in query:
            terms = query.split(" not ")

            if len(terms) == 2:
                term1 = terms[0].strip()
                term2 = terms[1].strip()

                if term1 in self.index and term2 in self.index:
                    result = self.index[term1].difference(
                        self.index[term2]
                    )

                    return sorted(result)

            return []

        elif query.startswith("not "):
            term = query[4:].strip()

            if term in self.index:
                all_documents = set(documents.keys())

                result = all_documents.difference(
                    self.index[term]
                )

                return sorted(result)

            return []

        elif query in self.index:
            return sorted(self.index[query])

        return []


if __name__ == "__main__":

    indexer = BooleanRetrieval()

    documents = {
        1: "Python is a programming language",
        2: "Information retrieval deals with finding information",
        3: "Boolean models are used in information retrieval"
    }

    for doc_id, text in documents.items():
        indexer.index_document(doc_id, text)

    indexer.create_documents_matrix(documents)
    indexer.print_documents_matrix_table()
    indexer.print_all_terms()

    query = input("\nEnter your boolean query: ")

    results = indexer.boolean_search(query)

    if results:
        print(f"Results for '{query}': {results}")
    else:
        print("No results found for the query.")
```

### Output:
<img width="1508" height="433" alt="image" src="https://github.com/user-attachments/assets/d09025e3-2cb1-4ef7-a52e-c2be4cc24925" />
<img width="1450" height="109" alt="image" src="https://github.com/user-attachments/assets/9be06ebc-0ca1-4184-b581-82521cea8e03" />
<img width="1605" height="118" alt="image" src="https://github.com/user-attachments/assets/54c688a2-8de2-48cc-92cb-574d5d2ca000" />

### Result:
Implementation of Information Retrieval Using Boolean Model in Python is successfully completed.


