# salesforce-sharepoint-integration

**1. Architectural Principles: **
The fundamental design principle governing this integration is the strict separation of duties between the two systems:
**• Salesforce:** It acts as the system of engagement, navigation, security, and metadata management. It controls who can access a document link but does not store the physical file long-term.
**• SharePoint:** It is the system of record for physical file storage, versioning, and retention. It holds the actual binaries,,.
**2. The Document Lifecycle Flow**
The architecture defines a specific lifecycle moving from ingestion to permanent archiving.
**Step 1: Ingestion (Upload & Staging)**
**• Entry Point: **Beneficiaries upload documents via the Client Portal (Experience Cloud) or Agents upload via the Salesforce internal interface.
**• Temporary Storage:** Files land initially in a Salesforce Staging area (temporary storage),.
**• Validation:** An internal reviewer (AGB/Agent) validates the attachments while they are in the staging phase,.
**Step 2: Promotion **
Once validated, the integration promotes the file from the temporary Salesforce storage to the permanent SharePoint storage.
**• Trigger:** A validation event or specific status change triggers the move.
**• Integration Layer:** The transfer is managed via an Integration layer.
**• Action:** The file binary is transferred to SharePoint Online.
**• Metadata Sync:** Simultaneously, the system writes/updates metadata in Salesforce (GUID, ObjectSFId, URL, DocID) to establish a permanent link to the file in SharePoint,.
**Step 3: Cleanup**
**• Deletion:** Immediately following the successful transfer to SharePoint, the file binary is deleted from Salesforce to optimize storage.
**3. Data Retrieval and Visualization**
**Internal Users (IQ Agents)**
**• No Direct Access:** Agents do not navigate SharePoint folder structures directly.
**• Salesforce Component: **They view documents through a custom Salesforce Document Component (Connector) that retrieves the content stream from SharePoint on demand.
**• Context:** Documents are visible within the context of the Account, Grant Application, or Disbursement records.
**External Users (Beneficiaries)**
**• Visibility:** Beneficiaries can see the titles of the documents they submitted (metadata in Salesforce) but generally cannot open or download the content after submission to ensure record integrity,.
**• Modification:** Documents are locked after submission; they can only be replaced or deleted while the application is in "Draft" status,.
