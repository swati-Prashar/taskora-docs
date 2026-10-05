---
icon: file
---

# Programmatic Content Migrations (Management API)

````markdown
# ⚙️ Programmatic Content Migrations (Management API)

This engineering layout document outlines how to execute write operations and scale data ingestion pipelines programmatically using a REST-based Management API layer.

### 🛠️ Production Code Blueprint: Automating Bulk Story Ingestion
While delivery APIs are read-only and cached, the Management API handles write tasks—allowing technical writers and engineers to build scripts that push legacy blogs, system assets, or text arrays straight into the cloud dashboard workspace programmatically.

```javascript
// scripts/bulkIngest.js
// Runs securely inside backend Node.js configuration script execution spaces.

async function executeMigrationNode() {
  const managementSecret = process.env.CMS_MANAGEMENT_TOKEN;
  const targetSpaceDirectoryId = process.env.CMS_SPACE_ID;

  const res = await fetch(`https://headlesscms.dev{targetSpaceDirectoryId}/stories`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': managementSecret
    },
    body: JSON.stringify({
      story: {
        name: "Automated Migration Payload",
        slug: "automated-migration-payload",
        content: {
          component: "article_layout",
          body_text: "Successfully parsed, cleaned, and programmatically injected from external legacy databases."
        }
      },
      publish: 1 // Instantly triggers production caching pipelines upon successful write tasks
    })
  });

  const data = await res.json();
  console.log(`Ingestion vector complete. Generated ID: ${data.story.id}`);
}
```

````
