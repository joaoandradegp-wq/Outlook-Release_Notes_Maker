<h1 align="center">📑 Outlook Release Notes Maker</h1>

<p align="center">
Automatically generates <b>Release Notes / GMUD</b> presentations from Outlook emails, transforming approval requests and source records into a ready-to-use PowerPoint presentation.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-In%20Development-brightgreen">
  <img src="https://img.shields.io/badge/Language-Node.js-green">
  <img src="https://img.shields.io/badge/Automation-Outlook-blue">
  <img src="https://img.shields.io/badge/Output-PowerPoint-orange">
  <img src="https://img.shields.io/badge/Python-PPTX-yellow">
  <img src="https://img.shields.io/badge/Documentation-English-lightgrey">
</p>

---

<h2>📌 About</h2>

<p>
<b>GMUD Release Notes Maker</b> is a tool designed to automate the creation of <b>GMUD</b> presentations using information available in <b>Outlook Web</b>.
</p>

<p>
The system accesses the mailbox through a browser, locates messages related to GMUD business approvals, identifies the selected reporting period, extracts requests listed in the <b>Source Record</b> field, and captures the corresponding approval requests and responses.
</p>

<p>
After collecting the information, the data is passed to a Python script that fills in a <b>PowerPoint (.pptx)</b> template while preserving the visual identity defined by the template.
</p>

<p align="center">
  <b>Outlook → GMUD Emails → Source Record → Requests → Approvals → PowerPoint</b>
</p>

---

<h2>🔄 How It Works</h2>

<pre>
┌──────────────┐     ┌───────────────┐     ┌─────────────────┐     ┌───────────────┐
│ Outlook Web  │ ──► │   index.js    │ ──► │ GMUD Emails and │ ──► │ Source Record │
│              │     │   Playwright  │     │ Approvals       │     │               │
└──────────────┘     └───────────────┘     └─────────────────┘     └───────┬───────┘
                                                                           │
                                                                           ▼
                                                                  ┌─────────────────┐
                                                                  │ Requests +      │
                                                                  │ Screenshots     │
                                                                  └────────┬────────┘
                                                                           │
                                                                           ▼
                                                                  ┌─────────────────┐
                                                                  │ gerar_pptx.py   │
                                                                  │ Python / PPTX   │
                                                                  └────────┬────────┘
                                                                           │
                                                                           ▼
                                                                  ┌─────────────────┐
                                                                  │   GMUD-YYYY-MM  │
                                                                  │      .pptx      │
                                                                  └─────────────────┘
</pre>

<p>
The entire workflow starts with <b>gerar_releasenotes.bat</b>. The user selects the desired month, and the script runs <b>index.js</b>.
</p>

<p>
Node.js opens Outlook Web through Playwright in headed mode, allowing the user to sign in manually when necessary. After authentication, the automation searches for and processes the matching emails, then calls Python to generate the presentation.
</p>

---

<h2>🚀 Key Features</h2>

<ul>
  <li>📧 Automatic email search in Outlook Web</li>
  <li>📅 Selection of the GMUD reporting month</li>
  <li>🔎 Search by configured subject and sender</li>
  <li>📋 Automatic reading of the <b>Source Record</b> field</li>
  <li>🧩 Extraction of requests / User Stories</li>
  <li>🔄 HTML content support with plain-text fallback</li>
  <li>📨 Automatic opening of the <b>Forward</b> workflow</li>
  <li>📜 Expansion of previous or collapsed email content</li>
  <li>🔎 Search for the GMUD deployment table</li>
  <li>📸 Screenshots of processed emails</li>
  <li>📤 Retrieval of the original approval request from <b>Sent Items</b></li>
  <li>🖼️ Capture of screenshots of the request and approval response</li>
  <li>📊 Automatic PowerPoint generation</li>
  <li>📑 Dynamic creation of request slides</li>
  <li>📝 Automatic normalization of User Story titles</li>
  <li>🖼️ Automatic insertion of approval images into the template</li>
  <li>🚫 Removal of empty slides</li>
  <li>🧪 Diagnostic files generated when extraction fails</li>
  <li>💾 Persistent browser session</li>
  <li>⚙️ Centralized configuration in the code</li>
</ul>

---

<h2>📂 Project Structure</h2>

<pre>
GMUD-Release_Notes/
│
├── gerar_releasenotes.bat
├── index.js
├── gerar_pptx.py
│
├── templates/
│   └── release_para_dev.pptx
│
├── .owa-session/
│   └── persistent Outlook session
│
└── output/
    ├── GMUD-YYYY-MM.pptx
    ├── config-pptx.json
    ├── resposta-0.png
    ├── pedido-0.png
    ├── debug-rascunho-0.png
    └── debug-rascunho-0.txt
</pre>

<p>
<b>index.js</b> handles Outlook automation, data extraction, and workflow orchestration.
</p>

<p>
<b>gerar_pptx.py</b> is responsible exclusively for building the presentation from the template.
</p>

<p>
The <b>templates</b> folder contains the PowerPoint layout used for generation, while the <b>output</b> folder receives the files created during execution.
</p>

---

<h2>🧩 Components</h2>

<h3>📧 index.js</h3>

<p>
This is the main application component and controls the entire automation workflow.
</p>

<ul>
  <li>🌐 Launches the browser with Playwright</li>
  <li>🔐 Accesses Outlook Web</li>
  <li>🔎 Searches emails</li>
  <li>📅 Defines the reporting period</li>
  <li>📋 Extracts requests</li>
  <li>📸 Captures screenshots</li>
  <li>📤 Searches for approval requests</li>
  <li>📦 Prepares the data for PowerPoint</li>
  <li>🐍 Runs the Python script</li>
</ul>

<p>
The browser starts with a persistent session in <b>.owa-session</b>, allowing the browser state to be reused between executions.
</p>

<h3>🤖 Playwright</h3>

<p>
Playwright controls Outlook Web through the browser.
</p>

<p>
The automation uses selectors compatible with both Portuguese and English interfaces to locate elements such as:
</p>

<ul>
  <li>🔎 Search</li>
  <li>📤 Forward</li>
  <li>📨 Send</li>
  <li>🗑️ Discard</li>
  <li>❌ Close</li>
  <li>✅ OK / Yes</li>
  <li>📁 Sent Items</li>
</ul>

<p>
This approach reduces dependence on a single Outlook interface language.
</p>

<h3>🐍 gerar_pptx.py</h3>

<p>
Generates the PowerPoint presentation using <b>python-pptx</b> and <b>Pillow</b>.
</p>

<p>
The script receives an intermediate JSON file containing the template, output filename, reporting period, requests, and approvals.
</p>

---

<h2>📅 Selecting the Reporting Period</h2>

<p>
The <b>gerar_releasenotes.bat</b> file asks the user to enter the month number for the report.
</p>

<pre>
==========================================
  GMUD - Generate PPTX
==========================================

Enter the month number (1 to 12).
Example: 9 = September.

Month:
</pre>

<p>
The value must be between <b>1 and 12</b>. Otherwise, the script asks the user to enter it again.
</p>

<p>
The month is passed to Node.js through the <b>GMUD_MES</b> environment variable.
</p>

<h3>📆 Determining the Year</h3>

<p>
If the year is not provided through the <b>GMUD_ANO</b> variable, the system determines the reference year automatically.
</p>

<ul>
  <li>📅 A month that has already occurred → current year</li>
  <li>🔄 A month that has not occurred yet → previous year</li>
</ul>

<p>
You can also set the year explicitly:
</p>

<pre>
set GMUD_MES=9
set GMUD_ANO=2026
</pre>

---

<h2>🔎 Email Search</h2>

<p>
After signing in to Outlook, the system automatically builds a query using:
</p>

<ul>
  <li>📌 Subject containing <b>de acordo de negocios gmud</b></li>
  <li>👤 Configured sender</li>
  <li>📅 First day of the selected month</li>
  <li>📅 Last day of the selected month</li>
</ul>

<p>
The query includes both received and sent messages within the selected period.
</p>

<pre>
subject:"de acordo de negocios gmud"
from:approver@yourcompany.com
(received:DD/MM/YYYY..DD/MM/YYYY
 OR sent:DD/MM/YYYY..DD/MM/YYYY)
</pre>

<p>
The number of matching emails is displayed in the console before individual processing begins.
</p>

---

<h2>📋 Request Extraction</h2>

<p>
For each matching email, the system looks for the content associated with the <b>Source Record</b> field.
</p>

<p>
It first attempts to extract the information directly from the email's HTML table structure.
</p>

<p>
If the HTML content cannot be found, a <b>fallback</b> strategy searches for the text directly in the message body.
</p>

<h3>🔄 Fallback Strategy</h3>

<pre>
Email
  │
  ├──► HTML table
  │       │
  │       └──► Source Record
  │
  └──► Plain text
          │
          └──► Source Record
</pre>

<p>
Request lines are identified using a pattern that accepts:
</p>

<ul>
  <li>🔢 A number with at least four digits</li>
  <li>📝 An optional <b>User Story</b> prefix</li>
  <li>🔤 A <b>:</b> or <b>-</b> separator</li>
</ul>

<p>Recognized examples:</p>

<pre>
12345: Process update
User Story 12345: New feature
12345 - Integration fix
</pre>

---

<h2>📤 Processing Through the Forward Action</h2>

<p>
When an email is opened, the system uses the <b>Forward</b> button to access a complete representation of the conversation content.
</p>

<p>The workflow tries several methods to locate and activate the button:</p>

<ul>
  <li>🔘 Portuguese menu item</li>
  <li>🔘 Portuguese button</li>
  <li>🔘 English menu item</li>
  <li>🔘 English button</li>
  <li>🔘 Text-based search</li>
  <li>🖱️ Forced click</li>
  <li>🖱️ Native DOM click</li>
</ul>

<p>
After the draft opens, the system waits for the table to load and the content to stabilize before extracting information.
</p>

---

<h2>🔄 Expanding Collapsed Content</h2>

<p>
Older Outlook conversations may contain hidden history behind controls such as <b>...</b> or options for displaying previous content.
</p>

<p>
The system attempts to expand this content automatically before searching for the <b>Source Record</b>.
</p>

<ul>
  <li>🔎 Finds the control for displaying previous content</li>
  <li>🖱️ Clicks the control</li>
  <li>⏳ Waits for the message body to stabilize</li>
  <li>🔁 Repeats the process up to the configured limit</li>
</ul>

<p>The maximum number of expansion rounds is controlled by:</p>

<pre>
rodadasExpansao: 3
</pre>

---

<h2>⏳ Loading Control</h2>

<p>
Because Outlook Web loads content dynamically, the system defines specific timeouts for each step.
</p>

<ul>
  <li><b>botaoEncaminhar</b>: maximum time to find the Forward button</li>
  <li><b>rascunhoAbrir</b>: maximum time for the draft to appear</li>
  <li><b>tabelaAparecer</b>: maximum time for the table to load</li>
  <li><b>corpoEstavelPor</b>: duration for which the content must remain stable</li>
  <li><b>corpoEstabilizar</b>: maximum time allowed for stabilization</li>
  <li><b>registroOrigem</b>: maximum time to find the Source Record</li>
  <li><b>rodadasExpansao</b>: maximum number of expansion attempts</li>
</ul>

<p>
These values can be adjusted directly in the <b>CONFIG.tempos</b> block in <b>index.js</b>.
</p>

---

<h2>📸 Screenshots</h2>

<p>
During processing, the system saves email screenshots for use in the report and for troubleshooting.
</p>

<h3>📨 Approval Response</h3>

<pre>
output/resposta-0.png
</pre>

<p>
This image represents the processed email and is used as the basis for the approval step.
</p>

<h3>📤 Approval Request</h3>

<pre>
output/pedido-0.png
</pre>

<p>
The system opens <b>Sent Items</b>, searches for a message with the same subject as the processed email, and captures the original request.
</p>

---

<h2>🧪 Diagnostics</h2>

<p>
If the system cannot locate the <b>Source Record</b>, diagnostic files are generated automatically.
</p>

<pre>
output/
├── debug-rascunho-0.png
└── debug-rascunho-0.txt
</pre>

<p>
The PNG file captures the page's visual state.
</p>

<p>
The TXT file contains text found in different page contexts, helping diagnose loading issues or changes to Outlook's structure.
</p>

<p>
This diagnostic layer is especially useful when Outlook changes its HTML structure or when conversation content takes longer than the configured timeout to appear.
</p>

---

<h2>📨 Finding the Approval Request</h2>

<p>
After extracting the requests, the system opens <b>Sent Items</b> to locate the corresponding approval request.
</p>

<p>
The search uses the same subject identified in the processed email.
</p>

<pre>
subject:"Email subject"
</pre>

<p>
When the message is found, the system opens the first result and saves:
</p>

<pre>
output/pedido-0.png
</pre>

<p>The approval slide is populated only when both files are available:</p>

<ul>
  <li>📤 Approval request screenshot</li>
  <li>📨 Approval response screenshot</li>
</ul>

---

<h2>📊 PowerPoint Generation</h2>

<p>
After processing the emails, Node.js consolidates the information into an intermediate JSON file.
</p>

<pre>
output/config-pptx.json
</pre>

<p>The file contains:</p>

<ul>
  <li><b>template</b>: path to the PPTX template</li>
  <li><b>output</b>: path to the final file</li>
  <li><b>mesAno</b>: reporting period displayed in the presentation</li>
  <li><b>demandas</b>: list of extracted requests</li>
  <li><b>aprovacoes</b>: pairs of request and response images</li>
</ul>

<p>
Node.js then runs the Python script and passes the JSON file as an argument.
</p>

<pre>
python gerar_pptx.py output/config-pptx.json
</pre>

---

<h2>🎨 PowerPoint Template</h2>

<p>The presentation is built from an existing template located at:</p>

<pre>
templates/release_para_dev.pptx
</pre>

<p>The template must contain four base slides:</p>

<ul>
  <li>📌 Slide 1: cover</li>
  <li>📋 Slide 2: request list</li>
  <li>📝 Slide 3: request slide template</li>
  <li>📸 Slide 4: approval slide template</li>
</ul>

<h3>🏷️ Shape Names</h3>

<p>The code searches for specific shape names in the template:</p>

<ul>
  <li><b>CaixaDeTexto 3</b> → cover title</li>
  <li><b>CaixaDeTexto 2</b> → request list</li>
  <li><b>CaixaDeTexto 5</b> → request title</li>
</ul>

<p>
Images on the approval slide are identified automatically by shape type and sorted horizontally.
</p>

<p>
The first image receives the approval request screenshot, and the second receives the approval response screenshot.
</p>

---

<h2>📝 User Story Formatting</h2>

<p>
Python automatically normalizes requests before inserting them into the presentation.
</p>

<p>
When a request has an identifiable number, it is formatted as follows:
</p>

<pre>
User Story 12345: Request name
</pre>

<p>
The <b>User Story 12345:</b> prefix and the remaining text are handled separately to preserve the visual formatting defined in the template.
</p>

---

<h2>📑 Dynamic Slides</h2>

<p>
The number of requests automatically determines how many slides are required.
</p>

<p>
The system uses the request slide as a template and creates copies for additional items.
</p>

<pre>
1 request
    ↓
1 request slide

5 requests
    ↓
5 request slides
</pre>

<p>
The same behavior applies to approval slides. Each approval creates an additional slide based on the template.
</p>

---

<h2>🚫 Empty Slides</h2>

<p>
If there are no requests, the request slide is removed from the presentation.
</p>

<p>
If there are no complete approvals, the approval slide is also removed.
</p>

<p>
As a result, the final presentation contains only sections with information to display.
</p>

---

<h2>📅 Output Filename</h2>

<p>
The PowerPoint filename is generated automatically based on the year and month being processed.
</p>

<pre>
GMUD-YYYY-MM.pptx
</pre>

<p>Example:</p>

<pre>
GMUD-2026-09.pptx
</pre>

<p>The file is saved in:</p>

<pre>
output/
</pre>

---

<h2>⚙️ Configuration</h2>

<p>
The main settings are defined in the <b>CONFIG</b> object inside <b>index.js</b>.
</p>

<ul>
  <li><b>userDataDir</b>: persistent browser session directory</li>
  <li><b>outlookUrl</b>: Outlook Web address</li>
  <li><b>destinatario</b>: sender used in the email search</li>
  <li><b>assuntoBusca</b>: text used in the Outlook query</li>
  <li><b>assuntoRegex</b>: expression used to validate the subject</li>
  <li><b>labelRegistroOrigem</b>: label identifying the field containing the requests</li>
  <li><b>regexLinhaDemanda</b>: pattern used to identify requests</li>
  <li><b>outputDir</b>: directory for generated files</li>
  <li><b>remetenteAprovador</b>: configured approver sender pattern</li>
  <li><b>tempos</b>: automation timeouts</li>
  <li><b>templatePptx</b>: PowerPoint template path</li>
  <li><b>gerarPptxScript</b>: Python script path</li>
  <li><b>pythonBin</b>: Python executable used for generation</li>
</ul>

---

<h2>🔐 Outlook Login</h2>

<p>
The system does not store usernames, passwords, or MFA details in the code.
</p>

<p>
When the browser opens, the user can sign in manually.
</p>

<pre>
>> Sign in manually in the opened browser
>> window if required.
>>
>> When the inbox has loaded, return here
>> and press ENTER to continue...
</pre>

<p>
Processing continues automatically after authentication.
</p>

<p>The browser session is stored in:</p>

<pre>
.owa-session/
</pre>

<p>
Using a persistent browser context allows the session state to be reused between executions.
</p>

---

<h2>🚀 Running the Application</h2>

<p>
On Windows, the recommended way to run the system is through:
</p>

<pre>
gerar_releasenotes.bat
</pre>

<p>The workflow is:</p>

<pre>
gerar_releasenotes.bat
        │
        ▼
Select month
        │
        ▼
Set GMUD_MES
        │
        ▼
node index.js
        │
        ▼
Sign in to Outlook
        │
        ▼
Search emails
        │
        ▼
Extract requests
        │
        ▼
Capture approvals
        │
        ▼
gerar_pptx.py
        │
        ▼
GMUD-YYYY-MM.pptx
</pre>

---

<h2>🧰 Dependencies</h2>

<h3>🟢 Node.js</h3>

<p>The Node.js component uses:</p>

<ul>
  <li>Node.js</li>
  <li>Playwright</li>
</ul>

<p>
Playwright controls the browser and automates Outlook Web.
</p>

<h3>🐍 Python</h3>

<p>The PowerPoint generator uses:</p>

<ul>
  <li>Python</li>
  <li>python-pptx</li>
  <li>Pillow</li>
</ul>

<p>
<b>python-pptx</b> manipulates the presentation, while <b>Pillow</b> is used to read, crop, and resize captured images.
</p>

---

<h2>🖼️ Image Processing</h2>

<p>
Screenshots used on approval slides are cropped before being inserted into PowerPoint.
</p>

<p>
For screenshots captured in a <b>1280 × 720</b> window, the crop area is:
</p>

<pre>
(775, 145, 1265, 460)
</pre>

<p>
If an image has a different resolution, the original image is used without this crop.
</p>

<p>
After cropping, the image is proportionally resized to fit the area defined in the template.
</p>

---

<h2>🛠️ Error Handling</h2>

<p>The system checks for several conditions during execution:</p>

<ul>
  <li>❌ Forward button not found</li>
  <li>❌ Forward draft did not open</li>
  <li>❌ Source Record not found</li>
  <li>❌ Approval email not found</li>
  <li>❌ PPTX template does not exist</li>
  <li>❌ Python executable not found</li>
  <li>❌ Error while running the PPTX generator</li>
  <li>⚠️ No valid emails processed</li>
</ul>

<p>
If no valid emails are processed, the PowerPoint presentation is not generated.
</p>

---

<h2>🔧 Configuring Python</h2>

<p>
On Windows, the Python executable path used by Node.js must be configured in <b>pythonBin</b>.
</p>

<pre>
pythonBin:
  process.platform === 'win32'
    ? 'C:\\path\\to\\python.exe'
    : 'python3'
</pre>

<p>
The path must point to a valid Python installation capable of running <b>gerar_pptx.py</b>.
</p>

---

<h2>📦 Data Flow</h2>

<p>
The project clearly separates data collection from presentation generation.
</p>

<pre>
OUTLOOK
   │
   ▼
index.js
   │
   ├── reporting period
   ├── emails
   ├── requests
   ├── screenshots
   └── approvals
   │
   ▼
config-pptx.json
   │
   ▼
gerar_pptx.py
   │
   ├── template
   ├── request list
   ├── individual slides
   └── approval images
   │
   ▼
GMUD-YYYY-MM.pptx
</pre>

<p>
The <b>config-pptx.json</b> file acts as the intermediate layer between Outlook automation and the PowerPoint generator.
</p>

---

<h2>🔤 Content Normalization</h2>

<p>
The system applies minor normalization to make the collected data suitable for the template.
</p>

<ul>
  <li>🧹 Removes HTML tags</li>
  <li>↩️ Converts HTML elements into text lines</li>
  <li>🔤 Removes unnecessary whitespace</li>
  <li>📋 Identifies request lines</li>
  <li>🏷️ Normalizes the User Story prefix</li>
  <li>📅 Formats the reporting period in Portuguese</li>
</ul>

<p>The cover period follows this format:</p>

<pre>
Setembro/26
</pre>

---

<h2>📋 Generated Files</h2>

<table>
<tr>
<th>File</th>
<th>Description</th>
</tr>
<tr>
<td><b>GMUD-YYYY-MM.pptx</b></td>
<td>Final presentation</td>
</tr>
<tr>
<td><b>config-pptx.json</b></td>
<td>Intermediate data passed to Python</td>
</tr>
<tr>
<td><b>resposta-N.png</b></td>
<td>Screenshot of the processed email</td>
</tr>
<tr>
<td><b>pedido-N.png</b></td>
<td>Screenshot of the approval request</td>
</tr>
<tr>
<td><b>debug-rascunho-N.png</b></td>
<td>Diagnostic screenshot</td>
</tr>
<tr>
<td><b>debug-rascunho-N.txt</b></td>
<td>Text captured during diagnostics</td>
</tr>
</table>

---

<h2>⚠️ Technical Notes</h2>

<p>
The system depends on the current structure of Outlook Web, especially the elements used for searching, forwarding, expanding message content, and navigating to <b>Sent Items</b>.
</p>

<p>
If Outlook changes its interface structure, the selectors used by Playwright may need to be updated.
</p>

<p>
Timeout values are centralized in <b>CONFIG.tempos</b> to make adjustments easier when Outlook loads more slowly.
</p>

<p>
The PowerPoint template is also part of the system's logic. Changing the names of shapes referenced by the code may prevent slides from being populated automatically.
</p>

---

<h2>🧪 Troubleshooting</h2>

<h3>❓ Source Record Not Found</h3>

<p>Check the following first:</p>

<ul>
  <li>Whether the conversation content has been fully expanded</li>
  <li>Whether the Forward button opened the draft correctly</li>
  <li>Whether the <b>registroOrigem</b> timeout is sufficient</li>
  <li>Whether the email still contains the <b>Source Record</b> label</li>
</ul>

<p>
Use <b>debug-rascunho-N.png</b> and <b>debug-rascunho-N.txt</b> to investigate the cause.
</p>

<h3>❓ PowerPoint Was Not Generated</h3>

<p>Check the following:</p>

<ul>
  <li>Whether at least one email was processed successfully</li>
  <li>Whether the <b>release_para_dev.pptx</b> template exists</li>
  <li>Whether Python is installed</li>
  <li>Whether the <b>pythonBin</b> path is correct</li>
  <li>Whether <b>python-pptx</b> and <b>Pillow</b> are available</li>
</ul>

<h3>❓ Approval Missing from PowerPoint</h3>

<p>
The approval slide requires both screenshots to be found:
</p>

<pre>
pedido-N.png
resposta-N.png
</pre>

<p>
If either image is missing, the corresponding pair is not added to the approval slides.
</p>

---

<h2>🛠️ Technologies</h2>

<ul>
  <li>Node.js</li>
  <li>JavaScript</li>
  <li>Playwright</li>
  <li>Outlook Web</li>
  <li>Python</li>
  <li>python-pptx</li>
  <li>Pillow</li>
  <li>PowerPoint / PPTX</li>
  <li>JSON</li>
  <li>Regular expressions (Regex)</li>
  <li>HTML</li>
</ul>

---

<p align="center">
<b>GMUD Release Notes Maker</b> automates the collection of requests and approvals directly from Outlook and transforms that information into a standardized PowerPoint presentation, reducing manual work when preparing GMUD materials while preserving the visual structure defined by the template. 📑
</p>
