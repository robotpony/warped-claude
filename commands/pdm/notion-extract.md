---                                                                                                         
name: pdm:notion-extract               
description: Extract a Notion page section into a local markdown doc
argument-hint: [notion-url] [topic-name]
---    
     
  1. Fetch the Notion page via MCP                                                                            
  2. If output is large (>20KB), delegate section extraction to an Explore agent with the topic name as search
   target — don't try to parse it inline                                                                      
  3. Create or update a local markdown file named after the topic
  4. Add source link, date extracted, and status section at top                                               
  5. Structure the content as an outline (headings, tables, key points) not a raw dump                        
  6. Find the current week's log and add a [[link]] under the appropriate day/section    