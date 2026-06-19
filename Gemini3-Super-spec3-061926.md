FDA 510(K) AGENTIC REVIEW STUDIO (V3.1 - SENTINEL)
Comprehensive Technical, Architectural, and Functional Specification Document
Author: Lead Principal Medical Software Architect
Version: 3.2.0-STABLE (Sentinel Extension Architecture)
Date: June 19, 2026
1. Executive Summary & System Vision
1.1 Scope of FDA 510(k) Submissions
The medical device premarket notification, widely known as the FDA 510(k) process under Section 510(k) of the Food, Drug, and Cosmetic Act, requires device sponsors to demonstrate that a new medical device is substantially equivalent (SE) to a legally marketed predicate device (Class I, II, or III not requiring premarket approval). An applicant must prove that the new device has the same intended use and the same technological characteristics, or that any differences in technological characteristics do not raise different questions of safety and effectiveness, and that the device is as safe and effective as the predicate.
Compiling, reviewing, and verifying the multi-volume submission package is a notoriously manual, error-prone, and capital-intensive endeavor. It demands deep cross-referencing against ever-changing Food and Drug Administration (FDA) guidance registries, 21 CFR (Code of Federal Regulations) standards, ISO/IEC requirements, and biocompatibility protocols. Gaps, ambiguities, or formatting misalignments often trigger "Refuse to Accept" (RTA) decisions or extensive "Additional Information" (AI) holds, delaying vital clinical equipment by months.
1.2 Purpose of the Agentic Review Studio
The FDA 510(k) Agentic Review Studio (v3.1 - Sentinel) is a high-precision, AI-powered regulatory workspace designed to streamline pre-submission reviews. The platform ingests draft regulatory dossier materials, structures unstructured documents (PDFs, Markdown, and ZIP directories), maps regulatory vectors, cross-references internal and external safety guidance files, and identifies structural inconsistencies using isolated LLM pipelines.
By operating under a fully deterministic "Sentinel" framework, the system is designed to catch critical gaps before materials are finalized. Its secondary mission is to empower regulatory affairs specialists to run "what-if" simulations, testing device descriptions against evolving standard baselines, translating dense parameters across languages, and compiling rigorous, interactive tables of contents.
1.3 Architectural & Design Philosophy: The Sentinel UI Paradigm
The user interface transitions from the dark-themed "Cosmic" layouts of previous developer iterations to the Sentinel Light Theme—a professional, high-contrast, clinical-grade interface. Drawing inspiration from pristine medical and technical review portals, it uses a highly legible typographical system pairing clean display headings (Space Grotesk / Playfair Display) with space-efficient mono accents (JetBrains Mono) for status tracking and telemetry indicators.
The design relies on generous negative space, crisp border distinctions (Slate-200/300), and interactive status banners. Status overlays such as "ONLINE" or "VERIFIED" are rendered in high-contrast solid tones rather than vibrating gradients. The application remains strictly client-centric or server-gated based on credential boundaries, discarding any unrequested telemetry clutter or promotional elements.
This specification articulates the complete structural design and technical blueprints required to incorporate five requested functional enhancements alongside three highly intelligent regulatory "Wow" AI sub-features.
2. Current System Architecture & UI Layout
The system utilizes a modular, full-stack React + TypeScript + Express architecture, unified through a single-page application (SPA) front-end and server-side API proxy.
code
Code
[ Client-Side Browser (Vite SPA) ]
                       │
       ┌───────────────┴───────────────┐
       ▼                               ▼
 [ Sentinel UI Layout ]         [ Recharts Engine ]
  ↳ DashboardPanel               ↳ Compliance Bars
  ↳ FileUploader                 ↳ Severity Pies
  ↳ GuidancePanel                ↳ Radar Confidences
  ↳ ReviewPanel                  ↳ Extracted Densities
  ↳ EditorPanels
  ↳ CoPilotPanel                       │
                       ▲               │ (API Proxy Requests)
                       │               ▼
       ┌───────────────┴───────────────┐
       ▼                               ▼
 [ Express Backend Server ] ──► [ Google Gemini SDK ]
  ↳ Directory Parsing            ↳ gemini-3.5-flash (Crawler)
  ↳ Zip / PDF Inspectors         ↳ gemini-3.1-pro (Auditor)
  ↳ Live Logger Stream
2.1 Front-End React Component Matrix
DashboardPanel.tsx: The mission-control dashboard. Hosts compliance score indicators, and coordinates real-time visualization cards for the regulatory readiness matrix, identified deficiency counts, and system performance metrics via custom-themed Recharts.
FileUploader.tsx: Handles multi-format package uploads. Supports asynchronous loading of PDFs, raw Markdown texts, and structured ZIP dossiers. Extracted files are parsed server-side and marked "READY" upon validation of compliance integrity.
GuidancePanel.tsx: Tracks user-configured search parameters. Coordinates queries against active FDA guidance guidelines to generate safety briefings. Supports prompt injection overrides and multi-language controls.
ReviewPanel.tsx: Compiles dossiers into exhaustive, structured Substantial Equivalence (SE) checklists. Cross-references regulatory variables against targeted predicates.
EditorPanels.tsx: Provides a split-pane interactive editing environment. Regulatory teams can directly manipulate baseline compliance criteria (skill.md) and modify the behavioral weights of underlying AI personas (agents.yaml).
CoPilotPanel.tsx: Houses conversational agents, high-density entity lists (20 structural nodes), bilingual legal translation mechanisms, and quick compliance short-cuts.
2.2 System Flow and State Management
Globally coordinated states (such as active files, parsed texts, console logs, active LLM selections, and language profiles) are propagated down from App.tsx through strict reactive paradigms. Side-effects, specifically asynchronous calls to Gemini LLM pipelines, are routed entirely through Express API routes, shielding API secrets from client-side inspectors.
3. Five requested Functional Enhancements
3.1 Feature 1: Multi-Format Document Export (PDF & DOCX)
To support standard, portable clinical and legal document exchanges, users require one-click exports of generated audit files, substantial equivalence matrices, and dossier summaries directly from the workspace.
code
Code
[Markdown Source] ──► [Structured Text Parser] ──► [Export Router]
                                                         │
                        ┌────────────────────────────────┴──────────────────┐
                        ▼                                                   ▼
             [ PDF Builder: jsPDF ]                              [ DOCX Builder: docx ]
   - Standard Letter-size grids                        - OpenXML semantic conversion
   - Header: "CONFIDENTIAL / FDA SUBMISSION"           - Nested tabular layout for SE comparisons
   - Footer: Dynamic Page Numbers & Timestamp          - Custom font family hierarchies
3.1.1 PDF Engine Implementation Details (jsPDF)
The export utility relies on jspdf to construct multi-page documents directly within the browser without server round-trips. Rather than relying on simple, fuzzy canvas-to-image dumps, the engine parses Markdown strings into standard typographies and layout vectors.
Page Setup & Grids: Enforces a standard US Letter page boundary (
, or 
), maintaining 
 margins (
) on all edges.
Font Hierarchies: Inter, Playfair Display, and JetBrains Mono fonts are dynamically mapped. Structural elements utilize the following schema:
Title (Doc Header): 22 pt, Bold, Slate-900 (RGB: 15, 23, 42)
H1 Header: 15 pt, Bold, Slate-800 (RGB: 30, 41, 59)
H2 Header: 12 pt, Semibold, Blue-600 (RGB: 37, 99, 235)
Body text: 10 pt, Regular, Slate-700 (RGB: 51, 65, 85) with a line-height multiplier of 1.45 (14.5 pt leading)
Mono metadata: 8 pt, JetBrains Mono, Slate-500 (RGB: 100, 116, 139)
Running Headers & Footers:
Header (First page excluded): Left-aligned document title; right-aligned "CONFIDENTIAL - MEDICAL DEVICE PRE-SUBMISSION" in 7 pt uppercase Slate-400. Includes a 0.5 pt solid horizontal rule at 
.
Footer (All pages): Left-aligned export timestamp; right-aligned page indexes rendered as Page X of Y via two-pass PDF generation (using the .putTotalPages() tokenizing method).
3.1.2 DOCX System Implementation Details (docx Library)
For editable documents destined for legal and medical regulatory portals, the export engine translates the parsed Markdown text tree into a structured docx file in standard OpenXML structure. This allows users to import generated documents directly into Microsoft Word without formatting anomalies.
Style Definitions: Incorporates a built-in OpenXML Styles block. Body Styles, List Bullet Styles, and Heading Categories map seamlessly to standard corporate layouts.
Nested Tabular Support: For substantial equivalence matrices, tabular data is programmatically formatted using Table, TableRow, and TableCell objects:
Alternating rows utilize a soft gray tint background (Slate-50: RGB F8FAFC).
Header cells are locked with vertical alignment, auto-fit boundaries, and soft blue accents (Blue-50/100).
Solid borders (1 pt) are rendered in a light Slate-200 frame to maintain document aesthetics.
Bullet & List Parser: Translates classic Markdown list items into native Word lists with consistent indentations (
 or 
).
code
TypeScript
// Architectural pseudo-code for Markdown to DOCX parsing transition
import { Document, Paragraph, TextRun, AlignmentType, HeadingLevel } from "docx";

function parseMarkdownToDocxParagraphs(markdownText: string): Paragraph[] {
  const lines = markdownText.split("\n");
  const paragraphs: Paragraph[] = [];

  for (let line of lines) {
    line = line.trim();
    if (line.startsWith("# ")) {
      paragraphs.push(new Paragraph({
        text: line.replace("# ", ""),
        heading: HeadingLevel.HEADING_1,
        spacing: { before: 240, after: 120 }
      }));
    } else if (line.startsWith("## ")) {
      paragraphs.push(new Paragraph({
        text: line.replace("## ", ""),
        heading: HeadingLevel.HEADING_2,
        spacing: { before: 180, after: 80 }
      }));
    } else if (line.startsWith("- ") || line.startsWith("* ")) {
      paragraphs.push(new Paragraph({
        bullet: { level: 0 },
        children: [new TextRun(line.substring(2))],
        spacing: { after: 60 }
      }));
    } else if (line.length > 0) {
      // Bold syntax extraction
      const boldRegex = /\*\*(.*?)\*\*/g;
      const children: TextRun[] = [];
      let lastIndex = 0;
      let match;

      while ((match = boldRegex.exec(line)) !== null) {
        if (match.index > lastIndex) {
          children.push(new TextRun(line.substring(lastIndex, match.index)));
        }
        children.push(new TextRun({ text: match[1], bold: true }));
        lastIndex = boldRegex.lastIndex;
      }
      if (lastIndex < line.length) {
        children.push(new TextRun(line.substring(lastIndex)));
      }

      paragraphs.push(new Paragraph({ children, spacing: { after: 120 } }));
    }
  }
  return paragraphs;
}
3.2 Feature 2: Editor Version History & Snapshot State Machine
The split-pane interactive playground within EditorPanels.tsx allows regulatory specialists to modify underlying guidelines (skill.md) and agent personas (agents.yaml). To safeguard against unintentional data loss or configuration errors, the editor incorporates a local, persistent version control state machine.
code
Code
[ User Edits skill.md / agents.yaml ] ──► [ Change Detection (1s Debounce) ]
                                                            │
                                                            ▼
                                                [ Generate Version Hash ]
                                                            │
                                                            ▼
                                              [ Create VersionSnapshot object ]
                                                            │
                                                            ▼
                                              ┌───────────────────────────┐
                                              ▼                           ▼
                                      [ State Memory ]         [ LocalStorage Sync ]
                                     - Active snapshots list    - key: `fda_studio_history_{file}`
                                     - History depth limit 50   - JSON serialized schema
3.2.1 State Representation & Persistent Database Schema
The state engine maintains individual historiographies for each target document. At every validated checkpoint, a signature fingerprint is computed.
code
TypeScript
interface VersionSnapshot {
  id: string;             // Unique cryptographic identifier (nanoid or UUID format)
  timestamp: string;      // ISO 8601 string (e.g. "2026-06-19T00:11:57Z")
  versionLabel: string;   // Human-readable index identifier (e.g., "v4 - Standardize Biocompatibility")
  content: string;         // Full text footprint
  byteSize: number;       // Content character-footprint
  fingerprint: string;    // SHA-1/SHA-256 slice calculated for integrity validation
  author: string;         // System role identifying action agent ("User Customization" or "System Standardizer")
}

interface LocalHistoryRegistry {
  currentActiveId: string;
  snapshots: VersionSnapshot[];
}
3.2.2 State Machine Transition Rules
Snapshot Creation Gating: Automatic background snapshots are captured via change debouncers when editing pauses for more than 
. This avoids capturing sequential keypresses.
Deduplication Validation: Before finalizing a snapshot, the system compares the updated content string's hash against the terminal active snapshot's fingerprint. If the contents are functionally identical, the duplicate registration is blocked.
Rolling Queue Limit: The version list enforces an upper limit of 
 per file. When snapshot count exceeds 50, the oldest records are pruned via first-out queues (FIFO) to conserve client-side LocalStorage space.
Rollback Mechanics: Selecting a historic point swaps the active state of the editor code canvas and runs a full lint evaluation. Rolling back adds an informational entry in the system logs.
code
TypeScript
// Architectural pseudo-code for Version Controller integration
export class VersionController {
  private fileKey: string;
  private limit: number = 50;

  constructor(fileName: 'skill.md' | 'agents.yaml') {
    this.fileKey = `fda_studio_history_${fileName.replace('.', '_')}`;
  }

  public getHistory(): LocalHistoryRegistry {
    const data = localStorage.getItem(this.fileKey);
    if (!data) {
      return { currentActiveId: "", snapshots: [] };
    }
    return JSON.parse(data);
  }

  public createSnapshot(content: string, author: string, comment: string): VersionSnapshot | null {
    const registry = this.getHistory();
    const hash = this.computeFnv1aHash(content);

    // Stop duplicates of identical state
    if (registry.snapshots.length > 0 && registry.snapshots[0].fingerprint === hash) {
      return null;
    }

    const newSnapshot: VersionSnapshot = {
      id: Math.random().toString(36).substring(2, 11),
      timestamp: new Date().toISOString(),
      versionLabel: comment || `Snapshot #${registry.snapshots.length + 1}`,
      content,
      byteSize: new Blob([content]).size,
      fingerprint: hash,
      author
    };

    const updatedSnapshots = [newSnapshot, ...registry.snapshots].slice(0, this.limit);
    
    const updatedRegistry: LocalHistoryRegistry = {
      currentActiveId: newSnapshot.id,
      snapshots: updatedSnapshots
    };

    localStorage.setItem(this.fileKey, JSON.stringify(updatedRegistry));
    return newSnapshot;
  }

  private computeFnv1aHash(str: string): string {
    let hash = 2166136261;
    for (let i = 0; i < str.length; i++) {
      hash ^= str.charCodeAt(i);
      hash += (hash << 1) + (hash << 4) + (hash << 7) + (hash << 8) + (hash << 24);
    }
    return (hash >>> 0).toString(16);
  }
}
3.3 Feature 3: Parallelized Batch Review Engine
Medical device portfolios often include separate attachments for software, hardware, biocompatibility, and packaging. Evaluating these documents individually introduces performance bottlenecks. The Batch Review Engine processes multiple dossiers in parallel, generating structured comparative reports and a unified compliance matrix.
code
Code
┌─────────┐   ┌─────────┐   ┌─────────┐
       │ File_A  │   │ File_B  │   │ File_C  │   ... (Multiple Dossier Packages)
       └────┬────┘   └────┬────┘   └────┬────┘
            │             │             │
            ▼             ▼             ▼
       ┌─────────────────────────────────────┐
       │ Multi-Queue Dossier Stream Manager  │
       └──────────────────┬──────────────────┘
                          │
            ┌─────────────┼─────────────┐
            ▼             ▼             ▼
       ┌─────────┐   ┌─────────┐   ┌─────────┐
       │ Pipeline│   │ Pipeline│   │ Pipeline│   (Process files concurrently)
       │    A    │   │    B    │   │    C    │
       └────┬────┘   └────┬────┘   └────┬────┘
            │             │             │
            ▼             ▼             ▼
       ┌─────────────────────────────────────┐
       │   Batch Comparative Aggrelator       │
       └──────────────────┬──────────────────┘
                          │
                          ▼
       ┌─────────────────────────────────────┐
       │ Multi-Submissions Synthesis Report  │
       └─────────────────────────────────────┘
3.3.1 Concurrent File Handling and Execution Orchestration
To avoid blocking the UI main thread while processing large documents, the Batch Engine schedules executions using asynchronous queue models.
Uploader Ingestion Interface: The upload area extends to accept multiple files simultaneously. Files are parsed, validating their document integrity before being routed to processing queues.
Concurrent Workers Pipeline:
In standard deployments, the system runs concurrent evaluation threads using Promise.allSettled() loops.
Separate instances of the Gemini 3.1 Advanced engine analyze each input stream using the baseline regulatory rules from skill.md. Each file is processed in isolation to prevent data leakage between submissions.
Data Shape of Comparative Summary Engine:
The results of each run are returned as individual JSON metrics payloads.
The comparative summary compiler processes these outputs to generate a side-by-side comparative markdown summary report comparing classifications, critical safety ratings, identified deficiency counts, and substantial equivalence scores.
code
TypeScript
interface BatchAnalysisResult {
  fileName: string;
  fileSize: string;
  deviceCategory: string;
  substantialEquivalenceScore: number;
  criticalGapsDetected: number;
  regulatoryCodeClass: string;
  findingsBrief: string;
}

interface BatchReportSummary {
  runTimestamp: string;
  overallComplianceGrade: string;
  documentsEvaluated: BatchAnalysisResult[];
  comparativeMarkdownDoc: string;
}
3.3.2 Parallel Queue Execution Management
code
TypeScript
async function executeBatchEvaluation(
  dossierFiles: { name: string; textContent: string }[],
  skillRules: string,
  onProgress: (fileName: string, phase: string) => void
): Promise<BatchReportSummary> {
  
  const tasks = dossierFiles.map(async (file) => {
    onProgress(file.name, "initiating parsing protocols...");
    try {
      // Step 1: Perform structural analysis via isolated LLM sandbox
      const analysisJson = await callIsolatedAnalysisModel(file.name, file.textContent, skillRules);
      onProgress(file.name, "synthesizing SE scores...");
      return {
        fileName: file.name,
        success: true,
        data: JSON.parse(analysisJson) as BatchAnalysisResult
      };
    } catch (err) {
      return {
        fileName: file.name,
        success: false,
        error: (err as Error).message
      };
    }
  });

  const results = await Promise.all(tasks);
  
  // Step 2: Extract active successes
  const activeEvaluations = results
    .filter(r => r.success && r.data)
    .map(r => r.data!);

  // Step 3: Run synthesis pipeline across successful extractions
  const comparativeMarkdownDoc = await compileSynthesisMarkdown(activeEvaluations, skillRules);

  return {
    runTimestamp: new Date().toISOString(),
    overallComplianceGrade: calculateAverageBatchGrade(activeEvaluations),
    documentsEvaluated: activeEvaluations,
    comparativeMarkdownDoc
  };
}
3.4 Feature 4: Interactive Recharts Drill-Down & Segment Filtering
The main system dashboard relies on Recharts to present high-level evaluations across the compliance, deficiency, and target radar components. Users can drill down into specific data segments to analyze the underlying data points contributing to overall scores.
code
Code
[ Compliance readiness charts or Severity Pie ]
                      │
                      ├─► user hover: show visual tooltips
                      │
                      └─► user mouse clicks (e.g., clicks 'Packaging' or 'High Severity' segment)
                                  │
                                  ▼
                     [ Trigger onMouseDown Event ]
                                  │
                                  ▼
                     [ Update dashboard Filter State ]
                                  │
                                  ▼
                   ┌──────────────────────────────┐
                   ▼                              ▼
          [ Reload Secondary Charts ]    [ Render filtered data lists ]
         - Isolates focus category      - Displays exact deficiencies
         - Updates coordinates          - Shows regulatory codes
3.4.1 Mechanics of Inter-Component Chart Filters
By binding onMouseDown and onCellClick triggers directly to Recharts child elements (<Cell />, <Bar />, <Pie />), the system captures precise data indexes. Clicking a visual segment updates the global dashboard filter state, dynamically updating all charts to focus on the selected category or severity level.
Category Isolation (Chart Zooming):
Selecting the Biocompatibility bar in the Compliance Readiness Matrix filters the Identified Deficiency Severity and Guidance Grounding Radar charts to display performance figures specifically for Biocompatibility.
Deficiency Isolation (Data Filtering):
Clicking the High Severity (Red) slice in the Deficiency Severity Pie focuses the terminal lists to show only critical deficiencies.
3.4.2 Visual Design & Interactive Telemetry
To signal interactivity, individual segments support micro-animations on hover (scaling up slightly and applying glow effects via custom inline SVGs). A dynamic breadcrumb utility at the top of the dashboard tracks the active filter hierarchy (e.g., All Submissions ➔ Biocompatibility ➔ High Severity Gaps), allowing specialists to revert to high-level system indices with a single click.
code
TypeScript
// Component integration model mapping interactive drill-downs
import { BarChart, Bar, Cell, XAxis, YAxis, CartesianGrid, Tooltip } from "recharts";

interface ReadinessData {
  name: string;   // e.g. "Software", "Biocompatibility", "Packaging"
  score: number;  // e.g. 85, 40, 92
  gaps: Array<{ id: string; title: string; severity: "High" | "Med" | "Low"; code: string }>;
}

export function InteractiveReadinessChart({
  data,
  onSelectCategory
}: {
  data: ReadinessData[];
  onSelectCategory: (categoryName: string) => void;
}) {
  return (
    <div className="h-56">
      <BarChart
        data={data}
        onMouseDown={(state) => {
          if (state && state.activeLabel) {
            onSelectCategory(state.activeLabel);
          }
        }}
        margin={{ top: 10, right: 10, left: -20, bottom: 5 }}
      >
        <CartesianGrid strokeDasharray="3 3" stroke="#e2e8f0" />
        <XAxis dataKey="name" stroke="#64748b" fontSize={11} />
        <YAxis stroke="#64748b" fontSize={11} domain={[0, 100]} />
        <Tooltip 
          cursor={{ fill: '#f1f5f9', opacity: 0.6 }}
          contentStyle={{ backgroundColor: "#ffffff", borderColor: "#e2e8f0", borderRadius: "8px" }}
        />
        <Bar dataKey="score" fill="#2563eb" radius={[4, 4, 0, 0]} className="cursor-pointer">
          {data.map((entry, index) => {
            // Apply conditional colors based on score levels
            const barColor = entry.score < 50 ? "#ef4444" : entry.score < 80 ? "#f59e0b" : "#10b981";
            return (
              <Cell 
                key={`cell-${index}`} 
                fill={barColor}
                className="hover:opacity-80 transition-opacity duration-150"
              />
            );
          })}
        </Bar>
      </BarChart>
    </div>
  );
}
3.5 Feature 5: Sentinel Quick Action Floating Menu (Overlay Dashboard)
For agile navigation during complex reviews, the interface incorporates a persistent, floating "Quick Action Menu" at the bottom-right corner of the primary viewport. The menu employs a modular concentric ring structure to prevent visual clutter.
code
Code
[ Base Circular Toggle Button: "Ω" ] ──► Clicks
                                             │
                                             ▼
                                [ Transition State change ]
                                             │
                                             ▼
                             [ Render Arc-Positions overlay ]
                                             │
                       ┌─────────────────────┼─────────────────────┐
                       ▼                     ▼                     ▼
          [ Run Risk Assessment ]   [ Validate Standards ]   [ Quick Compliance Scan ]
3.5.1 Layout, Positioning, and Interactive States
Desktop Positioning: Anchor position fixed at bottom: 24px, right: 24px with a high Z-index layer (z-50).
Mobile Adjustments: Anchors shift to a centered layout (bottom: 16px) when viewport widths fall below 648px to maintain touch margins.
Toggle State Motion: Clicks expand a series of concentric action buttons radially outwards. If closed, the system presents only a compact, high-contrast, slate-900 circular toggle utilizing the "Sentinel Ohm Ω" logotype.
Keyboard Shortcuts (Hotkeys):
Ctrl + Shift + R: Initiates active Risk Assessment.
Ctrl + Shift + V: Launches current standards verification checks.
Ctrl + Shift + S: Triggers a high-speed regulatory summary scanner.
code
TypeScript
// Positioning & Action Registry properties definition
interface QuickActionItem {
  id: string;
  iconName: string; // Map to dynamic Lucide icons (e.g. ShieldAlert, CheckCircle, Scale)
  title: string;
  actionTag: string;
  keyboardShortcut: string;
  colorPreset: string; // Tailwind colors (e.g. bg-blue-600)
}
4. Deep-dive: Three "Wow" AI Features
4.1 Wow Feature 1: Biocompatibility & Material Safety Predictor (ISO 10993)
This advanced feature automatically extracts the raw structural and material profiles of a device (such as stainless steels, titanium alloys, polyurethanes, color pigments, or processing lubricants) from uploaded documents, evaluating them against standard risk frameworks.
code
Code
[ Raw Uploaded text ] ──► [ Materials Extraction engine (Regex & LLM Schema) ]
                                                │
                                                ▼
                                    [ ISO 10993 Evaluator ]
                                 - Contact Type: Body, Tissue, Blood
                                 - Duration: Transitory, Prolonged, Permanent
                                                │
                                                ▼
                               ┌────────────────────────────────┐
                               ▼                                ▼
                     [ Grounding Checks ]            [ Toxicity & Allergy matrix ]
                     - FDA Guidance rules            - Sensitization risk database
                     - ASTM biocompatibilities       - Carcinogenicity indexes
                               │                                │
                               └────────────────┬───────────────┘
                                                ▼
                                  [ Biocompatibility briefing ]
                                 - PDF-exportable table
                                 - Prescribed physical safety trials
4.1.1 Biocompatibility Categorization Engine (Evaluation Rubric)
Device Contact Categories:
Surface-contacting: Skin, mucosal membranes, breached or compromised surfaces.
External communicating: Blood path, tissue/bone/dentin space, circulating blood.
Implant device: Tissue/bone, vascular structures.
Contact Duration Levels:
Limited (A): Transitory exposures under 24 hours.
Prolonged (B): Continuous or cumulative exposures between 24 hours and 30 days.
Permanent (C): Long-term exposures exceeding 30 days.
Safety Evaluation Profile:
Cross-references materials against ISO 10993 matrices to define mandatory testing profiles, including cytotoxicity, sensitization, irritation, systemic toxicity, subchronic toxicity, genotoxicity, and implantation effects.
code
TypeScript
// Materials schema extraction
interface MedicalMaterialNode {
  constituentChemical: string; // e.g. "Titanium Grade 5 (Ti-6Al-4V)"
  devicePartAssigned: string;  // e.g. "Patient-facing distal needle tip"
  contactCategory: "Surface" | "External Communicating" | "Implant";
  contactDuration: "Limited" | "Prolonged" | "Permanent";
  iso10993Requiredtests: string[];
  riskFactorScore: number;     // 1 to 10 safety threshold index calculated by LLM
  alternativesAdvised: string[];
}
4.1.2 Concrete Prompt Template for Gemini 3.1 Advanced Biocompatibility Analysis
code
Code
You are a Senior Principal Toxicologist and Biocompatibility Expert specializing in FDA 510(k) clearances and ISO 10993 standards. Your goal is to review the uploaded text to extract and evaluate all patient-contacting materials.

First, extract all materials mentioned in the device description, housing, packaging, or sterilizing sections.
For each extracted material, determine:
1. Contact Type (Surface, External Communicating, or Implant) based on the description of its functional use.
2. Contact Duration (Limited < 24h, Prolonged 24h-30d, or Permanent > 30d).
3. Under ISO 10993-1, compile the exact physical and chemical safeties required (e.g., Cytotoxicity, Sensitization, Intracutaneous Reactivity, Acute Systemic Toxicity).
4. Provide a structured risk-rating (1 = Safe, 10 = High Risk of Leachables/Degradation) and cite relevant FDA guidelines.

Represent your evaluations in a clear, parser-compliant JSON format matching this schema:
{
  "materialsExtracted": [
    {
      "constituentChemical": "Name",
      "devicePartAssigned": "Part",
      "contactCategory": "Category",
      "contactDuration": "Duration",
      "iso10993Requiredtests": ["Test1", "Test2"],
      "riskFactorScore": 5,
      "alternativesAdvised": ["Alt1"]
    }
  ],
  "overallToxicitySummary": "Brief overview of safety posture."
}

Do not include markdown blocks or HTML wrapping. Provide pure JSON.
4.2 Wow Feature 2: Predicate Device Semantic Alignment Matrix
Demonstrating Substantial Equivalence (SE) is the core objective of a 510(k) submission. This feature parses technical characteristics, mechanical tolerances, and operating paradigms from both the submission dossier and selected predicate classifications to flag substantial equivalence vulnerabilities.
code
Code
[ Submission Dossier Text ]              [ Target Predicate specs (Inputs) ]
             │                                              │
             ▼                                              ▼
 ┌──────────────────────────────────────────────────────────────┐
 │             Substantial Equivalence Parser                   │
 └──────────────────────────────┬───────────────────────────────┘
                                │
                                ▼
 ┌──────────────────────────────────────────────────────────────┐
 │             Predicate Semantic Alignment Matrix              │
 ├──────────────────────────────────────────────────────────────┤
 │  Intended Use Comparer                                       │
 │  Technological Contrast Index                                │
 │  Biocompatibility Delta Vector                               │
 └──────────────────────────────┬───────────────────────────────┘
                                │
                                ▼
 ┌──────────────────────────────────────────────────────────────┐
 │                SE Verdict Engine (LLM)                       │
 └──────────────────────────────┬───────────────────────────────┘
                                │
                                ▼
                       [ Audit Report ]
                      - Equivalence flags
                      - Safety gaps
                      - Corrective guidelines
4.2.1 Core Matrix Categories
Intended Use Comparer: Focuses on target indication vectors, patient populations, and treatment areas.
Technological Contrast Index: Analyzes operational characteristics, power parameters, software controls, and mechanical features.
Biocompatibility Delta Vector: Compares patient-contacting material compositions side-by-side.
Sterilization & Packaging Alignment: Cross-references sterilization methods (EtO, Gamma, Steam) and packaging configurations to identify performance gaps.
code
TypeScript
interface SECellPair {
  predicateValue: string;
  candidateValue: string;
  alignmentRating: "Identical" | "Equivalent" | "Discrepant" | "Incompatible";
  implicationAnalysis: string;
}

interface SEMatrixPayload {
  predicateKNumber: string;
  predicateCommercialName: string;
  matrixDimensions: {
    physicalFormFactor: SECellPair;
    operatingPrinciples: SECellPair;
    energySource: SECellPair;
    sensorsUtilized: SECellPair;
    materialsUsed: SECellPair;
  };
  gapsRiskSummary: string;
}
4.2.2 Concrete Prompt Template for Gemini 3.1 Advanced Semantic Alignment
code
Code
You are the primary Regulatory Compliance Expert running Substantial Equivalence Audits for the FDA 510(k) premarket notification branch. Your task is to process candidate and predicate text parameters to compile a comprehensive comparative matrix.

Candidate Text:
"""{CANDIDATE_TEXT}"""

Target Predicate Specs:
"""{PREDICATE_TEXT}"""

Compare the materials across five technical vectors: Physical Form Factor, Operating Principles, Energy Source, Sensors Utilized, and Materials Used. For each vector, extract values for both devices, assign a semantic correlation value (Identical, Equivalent, Discrepant, or Incompatible), and analyze whether variations introduce novel safety and efficacy questions.

Format output as high-density JSON:
{
  "predicateKNumber": "KXXXXXX",
  "predicateCommercialName": "Name",
  "matrixDimensions": {
    "physicalFormFactor": { "predicateValue": "", "candidateValue": "", "alignmentRating": "", "implicationAnalysis": "" },
    "operatingPrinciples": { "predicateValue": "", "candidateValue": "", "alignmentRating": "", "implicationAnalysis": "" },
    "energySource": { "predicateValue": "", "candidateValue": "", "alignmentRating": "", "implicationAnalysis": "" },
    "sensorsUtilized": { "predicateValue": "", "candidateValue": "", "alignmentRating": "", "implicationAnalysis": "" },
    "materialsUsed": { "predicateValue": "", "candidateValue": "", "alignmentRating": "", "implicationAnalysis": "" }
  },
  "gapsRiskSummary": "Summary details of discrepancies."
}
4.3 Wow Feature 3: Regulatory Path Finder & CFR 21 Code Advisor
Determining the classification of a medical device is a critical step in the regulatory process. This feature parses device functional descriptions, engineering parameters, and intentions to identify the appropriate FDA Product Code, classification regulation, and technical submission pathway.
code
Code
[ Input Device characteristics ] ──► [ Semantic Search across FDA Classification Indices ]
                                                          │
                                                          ▼
                                          [ Path Finder & Advisor ]
                                       - Predict Class: Class I, II, III
                                       - Regulation Number (21 CFR Part 800-898)
                                       - Extract Product Code (e.g. "DTI", "LFR")
                                                          │
                                                          ▼
                                      ┌───────────────────────────────────┐
                                      ▼                                   ▼
                             [ Pathway Predictor ]             [ Clinical Standard list ]
                             - Traditional 510(k)              - Consensus Standards
                             - Special 510(k)                  - Performance Controls
                             - Abbreviated 510(k)              - Performance Metrics
                             - De Novo Advisory                        │
                                      │                                │
                                      └────────────────┬───────────────┘
                                                       ▼
                                      [ Regulatory Pathway Checklist ]
                                     - Exemption rules
                                     - Technical requirements
4.3.1 Classification Database Structuring and Pathway Selection
Device Classification Rules:
Class I (General Controls): Minimum risk profile; often exempt from premarket notification.
Class II (Special Controls): Moderate risk profile; standard premarket notification path.
Class III (Premarket Approval): Patient-critical risk profiles; PMA process required.
21 CFR Regulation Subspace Identification:
Categorizes devices under applicable classification parts within Title 21 of the Code of Federal Regulations, including Anesthesiology, Cardiovascular, Dental, ENT, Gastroenterology-Urology, General Hospital, Neurological, Ophthalmic, Orthopedic, Physical Medicine, and Radiology.
Product Code Mapping:
Matches devices to three-letter FDA product codes to retrieve specific regulatory requirements, predicate options, and testing guidelines.
code
TypeScript
interface RegulatoryGuidelineBundle {
  fdaProductCode: string;           // e.g. "DTI" (Electrocardiograph)
  regulationNumber: string;         // e.g. "21 CFR 870.2340"
  deviceClass: "Class I" | "Class II" | "Class III";
  specialControlsRequired: string[];
  consensusStandardsISO: string[];
  recommendedSubmissionPathway: "Traditional" | "Special" | "Abbreviated" | "De Novo";
  exemptionRating: string;          // e.g. "Subject to 510(k) restrictions"
}
4.3.2 Concrete Prompt Template for Gemini 3.1 Advanced Class Advisor
code
Code
You are the Lead Master FDA Classification and Regulation Specialist. Your task is to analyze device specifications to map appropriate regulatory classifications, product codes, and legal pathways.

Device Functional Profile:
"""{DEVICE_PROFILE}"""

Task Requirements:
1. Identify the most specific three-letter FDA Product Code applicable to the device.
2. Formulate the exact 21 CFR regulation reference number (between 21 CFR 862.0000 and 21 CFR 892.0000).
3. Assign the regulatory safety classification (Class I, II, or III).
4. Outline the primary Consensus Standards (ISO, IEC, ASTM) the applicant must comply with (e.g. IEC 60601-1, ISO 14971).
5. Recommend the optimal 510(k) submission type (Traditional, Special, or Abbreviated) based on technological innovations.

Format the evaluation as JSON:
{
  "fdaProductCode": "XYZ",
  "regulationNumber": "21 CFR XXX.XXXX",
  "deviceClass": "Class II",
  "specialControlsRequired": ["Control 1", "Control 2"],
  "consensusStandardsISO": ["ISO XXXXX", "IEC XXXXX"],
  "recommendedSubmissionPathway": "Traditional",
  "exemptionRating": "Details"
}
5. Unified Database Schemas & State Interfaces
Integrating these requested and highly complex modules requires expanding the system's global state and database interfaces.
code
TypeScript
// Unified Local Database / State Model for the Extended Applet Workspace
export interface FDAWorkspaceState {
  // 1. Core Upload State
  activeDocuments: Array<{
    id: string;
    fileName: string;
    fileContent: string;
    fileType: string;
    fileSizeString: string;
  }>;

  // 2. Interactive Dashboard Metrics
  dashboardMetrics: {
    complianceScore: number;            // Overall workspace readiness rating (0 to 100)
    scoresByCategory: Array<{ 
      name: string; 
      score: number; 
      gapsLinked: Array<{ id: string; msg: string; severity: string; code: string }> 
    }>;
    deficienciesList: Array<{
      id: string;
      category: string;
      title: string;
      severity: "High" | "Medium" | "Low";
      exactCfrGapReference: string;
      correctiveRemedyInstruction: string;
    }>;
    groundingConfidenceScores: Array<{ topic: string; score: number }>;
    tokenPerformanceMetrics: Array<{ name: string; tokens: number }>;
  };

  // 3. Editor Snapshots State
  editorHistory: {
    skillMdHistory: {
      activeId: string;
      snapshots: VersionSnapshot[];
    };
    agentsYamlHistory: {
      activeId: string;
      snapshots: VersionSnapshot[];
    };
  };

  // 4. Batch Processing Queue state
  batchReviewPipeline: {
    isProcessing: boolean;
    queueProgress: Array<{ fileName: string; percentage: number; step: string }>;
    batchResults: BatchAnalysisResult[];
    synthesizedReport: string | null;     // Unified markdown synthesis output
  };

  // 5. Active Selections
  activeCategoryFilter: string | null;   // Null signifies displaying entire portfolio metrics
  selectedModel: string;
  languagePreference: "en" | "zh-tw";

  // 6. 3x Wow AI Outputs
  biocompatibilityProfile: {
    materialsList: MedicalMaterialNode[];
    globalSafetyVerdict: string;
  } | null;

  substantialEquivalenceMatrix: SEMatrixPayload | null;
  regulatoryCodeAdvisor: RegulatoryGuidelineBundle | null;
}
6. Prompt Engineering, Grounding, & Security Mandates
6.1 Strict Grounding & Zero Hallucination Guardrails
To enforce clinical precision, prevent hallucinated regulatory claims, and ensure strict compliance with actual legal baselines:
Predicate Anchoring: When referencing predicate documentation, the AI engine is restricted to retrieving factual device parameters, cleared intended uses, and verified technical dimensions. Speculative capabilities are flagged during pipeline runs.
Guideline Integration: All evaluations must directly map to instructions extracted from skill.md or loaded FDA guidance registries.
Reference Verifiability: When citing CFR sections, standards, or guidelines, the engine must supply valid, verifiable URLs or document identifiers (e.g., FDA Guidance for the Content of Premarket Submissions for Device Software Functions).
6.2 Patient Privacy, HIPAA & Sanitization Framings
Automatic De-identification (PHI Guard): All text processing pipelines incorporate built-in sanitization filters to strip Protected Health Information (PHI) before transmission to external LLM endpoints.
Protected Vectors:
Patient names, medical record numbers (MRNs), facility names, or physician details are replaced with functional tokens (e.g. [SANITIZED_PATIENT_ID]).
Technical engineering drawings, specific chemical formula files, and proprietary source code comments are restricted to localized pre-parsing threads to protect sensitive intellectual property.
7. Implementation Sequence & Dependency Registry
7.1 Required Dependencies (package.json Additions)
Implementing these enhancements requires the addition of verified libraries to the workspace environment.
code
JSON
{
  "dependencies": {
    "jspdf": "^2.5.1",
    "docx": "^8.5.0",
    "lucide-react": "^0.300.0",
    "recharts": "^2.10.3",
    "react-markdown": "^9.0.1",
    "canvas-confetti": "^1.6.0"
  },
  "devDependencies": {
    "@types/canvas-confetti": "^1.6.0"
  }
}
7.2 Implementation Phase Diagram
code
Code
PHASE 1: Foundations & Exports                             PHASE 2: Interactivity & State
┌─────────────────────────────────┐                       ┌─────────────────────────────────┐
│ - Install jsPDF / docx libraries│ ─────► ─────► ─────►  │ - Bind Recharts Click Handlers  │
│ - Integrate markdown-to-doc     │                       │ - Setup state filters dashboard │
│ - Add PDF & DOCX export routes  │                       │ - Build Floating Action Overlay │
└─────────────────────────────────┘                       └─────────────────────────────────┘
                                                                           │
                                                                           │
                                                                           ▼
PHASE 4: 3x Wow AI Modules                                 PHASE 3: Batch Review & History
┌─────────────────────────────────┐                       ┌─────────────────────────────────┐
│ - Biocompatibility Estimator    │                       │ - Concurrent Worker queue API   │
│ - Semantic Equivalence Finder   │  ◄───── ◄───── ◄───── │ - Comparative Markdown synthesis│
│ - CFR classification Advisor    │                       │ - LocalStorage Snapshot machine │
└─────────────────────────────────┘                       └─────────────────────────────────┘
8. Verification & Test Plan
To ensure overall platform stability, regression testing must confirm standard operations across both core and expanded features:
Formatting Engine Audits:
Verify that jspdf and docx export files render mathematical symbols, subscripts (e.g. 
), tables, and bold markdown tags without corrupting document metadata.
Version Controller Boundary Tests:
Confirm that the LocalStorage history queue safely prunes old snapshots when the maximum of 50 records is reached without causing buffer overflows.
Mock Concurrent Stress Tests:
Simulate the simultaneous upload of up to 10 distinct dossier packages. Validate that the runtime queues process files in isolation without cross-document data leakage.
Drill-down State Assertions:
Check that clicking on chart segments correctly filters dashboard items, and verify that selecting the "All Submissions" breadcrumb cleanly resets data tables.
Approval Signature:
FDA 510(k) Sentinel Architecture Validation Board
