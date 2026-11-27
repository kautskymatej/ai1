"use client";

/**
 * GitHub-ready React workflow map.
 * Dependencies: react, lucide-react.
 * Includes nine departments, 54 agent definitions, sample workflow definitions,
 * and styles. No project-specific imports or Tailwind setup are required.
 *
 * onSelectAgent opens your agent inspector.
 * onRunWorkflow connects to your workflow executor.
 * Pass runs and activeRunId from that executor to animate real active paths.
 * No simulated execution or API keys are included.
 */
import { useEffect, useState, type ComponentProps } from "react";
import {
  AudioLines,
  Bot,
  BriefcaseBusiness,
  ChartNoAxesCombined,
  ChevronRight,
  Code2,
  Compass,
  Headphones,
  Maximize,
  Megaphone,
  Minus,
  Network,
  Play,
  Plus,
  Settings2,
  Users,
} from "lucide-react";

export const departments = [
  {
    id: "executive",
    name: "Executive",
    description: "Strategy, priorities, and business decisions.",
    icon: Compass,
    color: "lime",
    roles: [
      "Chief of Staff",
      "Strategic Planner",
      "Decision Analyst",
      "Business Analyst",
      "Executive Assistant",
      "Performance Advisor",
    ],
  },
  {
    id: "marketing",
    name: "Marketing",
    description: "Turn ideas into campaigns that connect.",
    icon: Megaphone,
    color: "purple",
    roles: [
      "Growth Strategist",
      "Content Writer",
      "SEO Specialist",
      "Social Media Manager",
      "Campaign Planner",
      "Brand Strategist",
    ],
  },
  {
    id: "sales",
    name: "Sales",
    description: "Build relationships and grow your pipeline.",
    icon: BriefcaseBusiness,
    color: "blue",
    roles: [
      "Lead Researcher",
      "Sales Strategist",
      "Outreach Writer",
      "CRM Coordinator",
      "Proposal Writer",
      "Account Researcher",
    ],
  },
  {
    id: "finance",
    name: "Finance",
    description: "Make every number work for your business.",
    icon: ChartNoAxesCombined,
    color: "amber",
    roles: [
      "Financial Analyst",
      "Budget Planner",
      "Invoice Coordinator",
      "Forecast Analyst",
      "Expense Reviewer",
      "Revenue Analyst",
    ],
  },
  {
    id: "support",
    name: "Customer Success",
    description: "Keep customers informed and supported.",
    icon: Headphones,
    color: "pink",
    roles: [
      "Support Specialist",
      "Onboarding Guide",
      "Feedback Analyst",
      "Knowledge Curator",
      "Retention Strategist",
      "Customer Insights Analyst",
    ],
  },
  {
    id: "product",
    name: "Product & Engineering",
    description: "Plan, build, and improve your product.",
    icon: Code2,
    color: "teal",
    roles: [
      "Product Strategist",
      "Technical Architect",
      "Code Reviewer",
      "QA Analyst",
      "UX Researcher",
      "Release Coordinator",
    ],
  },
  {
    id: "people",
    name: "People & Culture",
    description: "Help your team do their best work.",
    icon: Users,
    color: "orange",
    roles: [
      "People Operations",
      "Hiring Coordinator",
      "Learning Advisor",
      "Culture Analyst",
      "Team Coordinator",
      "Policy Writer",
    ],
  },
  {
    id: "research",
    name: "Research",
    description: "Find the signals that inform your next move.",
    icon: AudioLines,
    color: "cyan",
    roles: [
      "Market Researcher",
      "Competitor Analyst",
      "Trend Researcher",
      "Data Analyst",
      "Industry Researcher",
      "Research Editor",
    ],
  },
  {
    id: "operations",
    name: "Operations",
    description: "Keep the work moving and the details covered.",
    icon: Settings2,
    color: "slate",
    roles: [
      "Workflow Coordinator",
      "Process Analyst",
      "Project Planner",
      "Resource Planner",
      "Quality Reviewer",
      "Documentation Writer",
    ],
  },
];

export type Agent = {
  id: string;
  name: string;
  department: string;
  color: string;
  initials: string;
  description: string;
  enabled: boolean;
};

export const agents: Agent[] = departments.flatMap((department) =>
  department.roles.map((name, index) => ({
    id: `${department.id}-${index}`,
    name,
    department: department.id,
    color: department.color,
    initials: name
      .split(" ")
      .slice(0, 2)
      .map((word) => word[0])
      .join(""),
    description: `Prepare ${name.toLowerCase()} drafts and recommendations for the ${department.name.toLowerCase()} team. Review source material, explain assumptions, and return an actionable output for human review.`,
    enabled: index !== 5,
  })),
);

export type Workflow = {
  id: string;
  name: string;
  description: string;
  department: string;
  agentIds: string[];
  enabled: boolean;
  steps: string[];
};
export const initialWorkflows: Workflow[] = [
  {
    id: "weekly-brief",
    name: "Weekly business brief",
    description: "Bring every department into one clear picture.",
    department: "executive",
    agentIds: ["executive-0", "finance-0", "operations-0"],
    enabled: true,
    steps: [
      "Gather department updates",
      "Summarize performance and priorities",
      "Prepare a brief for your review",
    ],
  },
  {
    id: "content-pipeline",
    name: "Content pipeline",
    description: "From a campaign idea to a ready-to-review draft.",
    department: "marketing",
    agentIds: ["marketing-0", "marketing-1", "marketing-2"],
    enabled: true,
    steps: [
      "Outline the campaign",
      "Draft the content",
      "Review search intent and messaging",
    ],
  },
  {
    id: "lead-research",
    name: "Lead research & qualification",
    description: "Organize account research for your sales team.",
    department: "sales",
    agentIds: ["sales-0", "sales-1", "sales-3"],
    enabled: true,
    steps: [
      "Review target account criteria",
      "Create a research checklist",
      "Prepare an outreach brief",
    ],
  },
  {
    id: "customer-feedback",
    name: "Customer feedback digest",
    description: "Turn feedback into a focused product backlog.",
    department: "support",
    agentIds: ["support-2", "support-4", "product-0"],
    enabled: false,
    steps: [
      "Group feedback by theme",
      "Assess recurring requests",
      "Draft product recommendations",
    ],
  },
];

export type Run = {
  id: string;
  workflowId: string;
  workflowName: string;
  createdAt: string;
  status: "running" | "review" | "approved" | "failed" | "cancelled";
  output: string;
  sample: boolean;
  engine?: "tools" | "ai";
  input?: string;
  durationMs?: number;
  error?: string;
  activeAgentId?: string;
};

type ButtonProps = ComponentProps<"button"> & {
  size?: "sm" | "icon";
  variant?: "outline";
};
function Button({
  size,
  variant,
  className = "",
  type = "button",
  ...props
}: ButtonProps) {
  return (
    <button
      {...props}
      type={type}
      className={`wf-button ${size || ""} ${variant || ""} ${className}`}
    />
  );
}

const workflowStyles = `
.wf-export { --border:#4c3541; --primary:#d3b784; --ring:#e6cba0; color:#f2ede6; font:14px/1.6 system-ui,sans-serif; border:1px solid var(--border); border-radius:15px; max-width:100%; background:#140b10; }
.wf-export * { box-sizing:border-box; }
.wf-export button, .wf-export select { font:inherit; color:inherit; cursor:pointer; }
.wf-export button { background:transparent; border:0; transition:background 160ms,color 160ms,transform 160ms; }
.wf-export button:disabled { opacity:.45; cursor:not-allowed; }
.wf-export button:active:not(:disabled) { transform:scale(.97); }
.wf-export :focus-visible { outline:2px solid var(--ring); outline-offset:4px; }
.wf-export select { border:1px solid var(--border); background:#190c12; border-radius:8px; min-height:44px; padding:9px 12px; }
.wf-export select option { background:#190c12; color:#f2ede6; }
.wf-export .sr-only { position:absolute; width:1px; height:1px; padding:0; margin:-1px; overflow:hidden; clip:rect(0,0,0,0); white-space:nowrap; border:0; }
.wf-export .wf-button { display:inline-flex; align-items:center; justify-content:center; gap:8px; min-height:44px; padding:9px 15px; border:1px solid #d3b784; border-radius:8px; background:#d3b784; color:#21180e; font-weight:500; white-space:nowrap; }
.wf-export .wf-button svg { width:16px; height:16px; }
.wf-export .wf-button.outline { background:#25121b; color:#e7cda7; border-color:#684553; }
.wf-export .wf-button.icon { width:44px; padding:0; }
.wf-export .wf-button:hover:not(:disabled) { background:#e6c99a; color:#21180e; transform:translateY(-1px); }
.wf-export .status-dot { width:6px; height:6px; border-radius:50%; background:#e1c493; }
.system-workflow {
  overflow: hidden;
}
.workflow-map-toolbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 16px;
  padding: 18px 22px;
  border-bottom: 1px solid var(--border);
}
.map-breadcrumbs {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 9px;
  color: #aab1bd;
  font-size: 13px;
}
.map-breadcrumbs button {
  min-height: 36px;
  color: #d9e1d1;
}
.map-breadcrumbs button:hover {
  color: var(--primary);
}
.map-breadcrumbs > span {
  color: var(--primary);
}
.map-run-controls {
  display: flex;
  align-items: center;
  gap: 10px;
}
.map-run-controls select {
  width: auto;
  max-width: 270px;
  background: #111418;
  border: 1px solid var(--border);
  border-radius: 7px;
  padding-right: 16px;
  font-size: 12px;
}
.workflow-map-status {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 8px;
  padding: 14px 22px;
  color: #bbc3b4;
  font-size: 12px;
  background: #b9ef7003;
}
.workflow-map-status > span:last-child {
  margin-left: auto;
  color: #a0a5b1;
}
.map-idle-dot {
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: #9ca78f;
}
.workflow-map-viewport {
  height: min(72vh, 760px);
  min-height: 520px;
  overflow: auto;
  background: #101318;
  overscroll-behavior: contain;
}
.workflow-map-plane {
  min-width: 740px;
  min-height: 520px;
}
.workflow-map-svg {
  width: 100%;
  height: 100%;
  display: block;
}
.map-scene {
  transform-origin: 560px 400px;
  animation: map-focus-in 260ms ease-out;
}
@keyframes map-focus-in {
  from {
    opacity: 0.3;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}
.map-orbit {
  fill: none;
  stroke: #94a1b313;
  stroke-dasharray: 3 9;
}
.map-inner-orbit {
  fill: none;
  stroke: #b9ef7014;
}
.map-link {
  fill: none;
  stroke: currentColor;
  stroke-width: 1.7;
  opacity: 0.35;
}
.map-path-active .map-link {
  stroke: #b9ef70;
  opacity: 0.85;
  stroke-width: 2.8;
}
.map-flow-dot {
  fill: #e6ffc5;
  filter: drop-shadow(0 0 4px #b9ef70);
}
.map-flow-dot-soft {
  opacity: 0.6;
}
.map-node,
.map-root {
  cursor: pointer;
}
.map-node-ring {
  fill: #101318;
  stroke: currentColor;
  stroke-width: 1;
  opacity: 0.45;
}
.map-node-circle {
  fill: #191f27;
  stroke: currentColor;
  stroke-width: 1.5;
}
.map-node:hover .map-node-circle,
.map-node:focus-visible .map-node-circle {
  fill: #28323c;
  stroke-width: 2.5;
}
.map-node:focus-visible .map-node-ring {
  opacity: 1;
  stroke: #eef8e4;
  stroke-width: 3;
}
.map-node-active .map-node-ring {
  stroke: #b9ef70;
  opacity: 1;
  stroke-width: 2.5;
}
.map-node-title {
  fill: #e3e8f0;
  font:
    500 14px "Geist Variable",
    sans-serif;
}
.map-node-subtitle {
  fill: #a5aebb;
  font:
    12px "Geist Variable",
    sans-serif;
}
.map-root {
  color: #c5f197;
}
.map-root-ring {
  fill: #b9ef7009;
  stroke: #b9ef7055;
  stroke-width: 1.2;
}
.map-root-circle {
  fill: #23301b;
  stroke: #b9ef70;
  stroke-width: 1.5;
  filter: drop-shadow(0 0 14px #b9ef7010);
}
.map-root:hover .map-root-circle,
.map-root:focus-visible .map-root-circle {
  fill: #334626;
  stroke-width: 3;
}
.map-root-title {
  fill: #eaffd7;
  font:
    550 14px "Geist Variable",
    sans-serif;
}
.map-root-subtitle {
  fill: #b9c6ad;
  font:
    13px "Geist Variable",
    sans-serif;
}
.workflow-map-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  padding: 20px 22px;
  border-top: 1px solid var(--border);
}
.workflow-map-footer strong {
  font-size: 14px;
  font-weight: 550;
}
.workflow-map-footer p {
  font-size: 13px;
  color: #a4adbb;
  margin-top: 5px;
}
.map-zoom-controls {
  display: flex;
  align-items: center;
  gap: 8px;
}
.map-zoom-controls button {
  min-width: 44px;
  min-height: 44px;
}
.map-zoom-controls output {
  min-width: 45px;
  text-align: center;
  font-size: 12px;
  color: #c6cdd7;
  font-variant-numeric: tabular-nums;
}
.map-accessible-navigation {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 0 22px 20px;
}
.map-accessible-navigation label {
  color: #a5adba;
  font-size: 12px;
}
.map-accessible-navigation select {
  width: auto;
  border: 1px solid var(--border);
  border-radius: 7px;
  font-size: 12px;
  padding-right: 15px;
}
.wf-export .color-lime {
  background: #b9ef7014;
  color: #b9ef70;
}
.wf-export .color-purple {
  background: #a28afb17;
  color: #b7a2fa;
}
.wf-export .color-blue {
  background: #7ca7ff17;
  color: #96b8ff;
}
.wf-export .color-amber {
  background: #ecc27b15;
  color: #efca8e;
}
.wf-export .color-pink {
  background: #e297c017;
  color: #e9aed0;
}
.wf-export .color-teal {
  background: #73d4b417;
  color: #8dd8be;
}
.wf-export .color-orange {
  background: #e8a37b17;
  color: #e8b18f;
}
.wf-export .color-cyan {
  background: #73c8dc17;
  color: #94d6e7;
}
.wf-export .color-slate {
  background: #bbc5da17;
  color: #c5cbdb;
}
/* Keep the full interactive map, with the customer finish. */
.wf-export.system-workflow {
  background: #140b10;
  border-color: #4c3541;
  border-radius: 15px;
}
.wf-export .workflow-map-viewport {
  background: #10090c;
  height: min(76vh, 800px);
  min-height: 620px;
}
.wf-export .workflow-map-plane {
  min-width: 960px;
  min-height: 620px;
}
.wf-export .workflow-map-toolbar {
  border-bottom-color: #47303d;
  padding: 22px;
}
.wf-export .workflow-map-status {
  background: #201016;
  color: #d0b8a6;
}
.wf-export .map-run-controls select {
  background-color: #190c12;
  border-color: #573d4a;
  font-size: 14px;
}
.wf-export .map-accessible-navigation label,
.wf-export .map-accessible-navigation select {
  font-size: 14px;
}
.wf-export .map-breadcrumbs button,
.wf-export .map-breadcrumbs > span {
  color: #dfc499;
}
.wf-export .map-root {
  color: #e4c699;
}
.wf-export .map-root-ring {
  fill: #d3b7840b;
  stroke: #d3b78460;
}
.wf-export .map-root-circle {
  fill: #34141f;
  stroke: #d3b784;
}
.wf-export .map-root:hover .map-root-circle,
.wf-export .map-root:focus-visible .map-root-circle {
  fill: #4b1e2c;
}
.wf-export .map-root-title {
  fill: #f3debd;
}
.wf-export .map-root-subtitle {
  fill: #c8b49f;
}
.wf-export .workflow-map-svg .color-lime,
.wf-export .workflow-map-svg .color-amber,
.wf-export .workflow-map-svg .color-blue,
.wf-export .workflow-map-svg .color-teal,
.wf-export .workflow-map-svg .color-slate {
  color: #d1b385;
}
.wf-export .workflow-map-svg .color-purple,
.wf-export .workflow-map-svg .color-pink,
.wf-export .workflow-map-svg .color-orange,
.wf-export .workflow-map-svg .color-cyan {
  color: #c19499;
}
.wf-export .map-node-circle {
  fill: #241017;
}
.wf-export .map-node-ring {
  fill: #10090c;
}
.wf-export .map-node:hover .map-node-circle,
.wf-export .map-node:focus-visible .map-node-circle {
  fill: #3d1926;
}
.wf-export .map-node-active .map-node-ring,
.wf-export .map-path-active .map-link {
  stroke: #e1c493;
}
.wf-export .map-flow-dot {
  fill: #fae1ba;
  filter: drop-shadow(0 0 4px #c8a77b);
}
.wf-export .map-inner-orbit {
  stroke: #d3b78420;
}
.wf-export .map-link {
  opacity: 0.5;
}
.wf-export .map-node-title {
  fill: #eee0d9;
}
.wf-export .map-node-subtitle {
  fill: #bca6af;
}
.wf-export .workflow-map-footer {
  border-top-color: #45323e;
}

.wf-export #workflow-glow stop { stop-color:#cbae7c; }
.wf-export .map-orbit { stroke:#a88a6624; }
@media(max-width:767px) { .wf-export .workflow-map-toolbar,.wf-export .workflow-map-footer,.wf-export .map-accessible-navigation { flex-wrap:wrap; } .wf-export .map-run-controls { width:100%; flex-wrap:wrap; } .wf-export .map-run-controls select { flex:1; max-width:none; min-width:170px; } .wf-export .workflow-map-viewport { min-height:570px; } .wf-export .workflow-map-plane { min-width:980px; } }
@media(prefers-reduced-motion:reduce) { .wf-export *, .wf-export *::before,.wf-export *::after { animation:none!important; transition:none!important; scroll-behavior:auto!important; } }
`;

function labelLines(label: string, maxLength = 23) {
  const words = label.split(" ");
  const lines = [""];
  for (const word of words) {
    const last = lines.length - 1;
    if (`${lines[last]} ${word}`.trim().length > maxLength && lines[last])
      lines.push(word);
    else lines[last] = `${lines[last]} ${word}`.trim();
  }
  return lines;
}

export function SystemWorkflow({
  workforce = agents,
  workflows = initialWorkflows,
  runs = [],
  activeRunId,
  businessName,
  busy = false,
  onSelectAgent,
  onRunWorkflow,
}: {
  workforce?: Agent[];
  workflows?: Workflow[];
  runs?: Run[];
  activeRunId?: string | null;
  businessName?: string;
  busy?: boolean;
  onSelectAgent: (agent: Agent) => void;
  onRunWorkflow: (workflow: Workflow) => void;
}) {
  const [departmentId, setDepartmentId] = useState<string | null>(null);
  const [zoom, setZoom] = useState(1);
  const [selectedWorkflow, setSelectedWorkflow] = useState(
    workflows[0]?.id || "",
  );
  const [motionAllowed, setMotionAllowed] = useState(false);
  const department = departments.find((item) => item.id === departmentId);
  const selected = workflows.find(
    (workflow) => workflow.id === selectedWorkflow,
  );
  const activeAgentIds = new Set(
    runs
      .filter((run) => run.status === "running" && run.id === activeRunId)
      .flatMap((run) => (run.activeAgentId ? [run.activeAgentId] : [])),
  );
  const activeDepartments = new Set(
    [...activeAgentIds].map(
      (id) => workforce.find((agent) => agent.id === id)?.department,
    ),
  );

  useEffect(() => {
    const media = window.matchMedia?.("(prefers-reduced-motion: reduce)");
    const update = () =>
      setMotionAllowed(
        !media?.matches && document.visibilityState === "visible",
      );
    update();
    media?.addEventListener("change", update);
    document.addEventListener("visibilitychange", update);
    return () => {
      media?.removeEventListener("change", update);
      document.removeEventListener("visibilitychange", update);
    };
  }, []);

  function focus(id: string | null) {
    setDepartmentId(id);
    setZoom(1);
  }
  const children = department
    ? workforce
        .filter((agent) => agent.department === department.id)
        .map((agent) => ({
          id: agent.id,
          title: agent.name,
          subtitle: agent.enabled ? "Ready to run" : "Paused",
          color: agent.color,
          active: activeAgentIds.has(agent.id),
          select: () => onSelectAgent(agent),
        }))
    : departments.map((item) => ({
        id: item.id,
        title: item.name,
        subtitle: item.roles[0],
        color: item.color,
        active: activeDepartments.has(item.id),
        select: () => focus(item.id),
      }));
  const centerTitle = department?.name || "Executive agent";
  const visibleActiveCount = children.filter((child) => child.active).length;
  const centerSubtitle = department
    ? "Department lead"
    : businessName || "Your business command center";

  return (
    <section
      className="system-workflow panel wf-export"
      aria-label="Interactive system workflow"
    >
      <style>{workflowStyles}</style>
      <div className="workflow-map-toolbar">
        <div className="map-breadcrumbs">
          <button
            onClick={() => focus(null)}
            aria-current={!department ? "page" : undefined}
          >
            Entire workforce
          </button>
          {department && (
            <>
              <ChevronRight size={14} aria-hidden="true" />
              <span>{department.name}</span>
            </>
          )}
        </div>
        <div className="map-run-controls">
          <label className="sr-only" htmlFor="map-workflow">
            Workflow to run
          </label>
          <select
            id="map-workflow"
            value={selectedWorkflow}
            onChange={(event) => setSelectedWorkflow(event.target.value)}
          >
            {workflows.map((workflow) => (
              <option key={workflow.id} value={workflow.id}>
                {workflow.name}
              </option>
            ))}
          </select>
          <Button
            size="sm"
            disabled={busy || !selected?.enabled}
            onClick={() => {
              if (selected) onRunWorkflow(selected);
            }}
          >
            <Play aria-hidden="true" />
            Run workflow
          </Button>
        </div>
      </div>
      <div className="workflow-map-status" role="status">
        <span className={activeAgentIds.size ? "status-dot" : "map-idle-dot"} />
        {visibleActiveCount
          ? `${visibleActiveCount} active path${visibleActiveCount > 1 ? "s" : ""}`
          : activeAgentIds.size
            ? "Execution is active in another department"
            : activeRunId
              ? "Preparing this run…"
              : "Ready · Run an agent or workflow to activate a path"}
        <span>
          {department
            ? `${children.length} subagents`
            : "9 departments · 54 roles"}
        </span>
      </div>
      <div
        className="workflow-map-viewport"
        tabIndex={0}
        aria-label="Workflow canvas. Use the zoom buttons and scroll to explore."
      >
        <div
          className="workflow-map-plane"
          style={{ width: `${zoom * 100}%`, height: `${zoom * 100}%` }}
        >
          <svg
            viewBox="0 0 1120 800"
            className="workflow-map-svg"
            role="group"
            aria-label={`${centerTitle} and its ${children.length} branches`}
          >
            <defs>
              <pattern
                id="workflow-grid"
                x="0"
                y="0"
                width="24"
                height="24"
                patternUnits="userSpaceOnUse"
              >
                <circle cx="1" cy="1" r="0.8" fill="#788397" opacity="0.16" />
              </pattern>
              <radialGradient id="workflow-glow">
                <stop offset="0%" stopColor="#d3b784" stopOpacity="0.06" />
                <stop offset="100%" stopColor="#d3b784" stopOpacity="0" />
              </radialGradient>
            </defs>
            <rect width="1120" height="800" fill="url(#workflow-grid)" />
            <circle cx="560" cy="400" r="320" fill="url(#workflow-glow)" />
            <g key={departmentId || "all"} className="map-scene">
              <circle cx="560" cy="400" r="292" className="map-orbit" />
              <circle cx="560" cy="400" r="105" className="map-inner-orbit" />
              {children.map((child, index) => {
                const angle =
                  -Math.PI / 2 + (index * Math.PI * 2) / children.length;
                const x = 560 + Math.cos(angle) * 292,
                  y = 400 + Math.sin(angle) * 292;
                const path = `M 560 400 Q ${560 + Math.cos(angle - 0.2) * 160} ${400 + Math.sin(angle - 0.2) * 160} ${x} ${y}`;
                return (
                  <g
                    key={`path-${child.id}`}
                    className={`map-connection color-${child.color} ${child.active ? "map-path-active" : ""}`}
                    aria-hidden="true"
                  >
                    <path d={path} className="map-link" />
                    {child.active && motionAllowed && (
                      <>
                        <circle r="4" className="map-flow-dot">
                          <animateMotion
                            dur="1.7s"
                            repeatCount="indefinite"
                            path={path}
                          />
                        </circle>
                        <circle
                          r="3"
                          className="map-flow-dot map-flow-dot-soft"
                        >
                          <animateMotion
                            dur="1.7s"
                            begin="0.85s"
                            repeatCount="indefinite"
                            path={path}
                          />
                        </circle>
                      </>
                    )}
                  </g>
                );
              })}
              <g
                className="map-root"
                role="button"
                tabIndex={0}
                aria-label={
                  department
                    ? `Open ${department.roles[0]}`
                    : "Open executive agent"
                }
                onClick={() =>
                  onSelectAgent(
                    workforce.find(
                      (agent) =>
                        agent.id === `${department?.id || "executive"}-0`,
                    ) || agents[0],
                  )
                }
                onKeyDown={(event) => {
                  if (["Enter", " "].includes(event.key)) {
                    event.preventDefault();
                    onSelectAgent(
                      workforce.find(
                        (agent) =>
                          agent.id === `${department?.id || "executive"}-0`,
                      ) || agents[0],
                    );
                  }
                }}
              >
                <circle cx="560" cy="400" r="76" className="map-root-ring" />
                <circle cx="560" cy="400" r="64" className="map-root-circle" />
                <Network
                  x="543"
                  y="364"
                  width="34"
                  height="34"
                  aria-hidden="true"
                />
                <text
                  x="560"
                  y="413"
                  textAnchor="middle"
                  className="map-root-title"
                >
                  {labelLines(centerTitle, 16).map((line, index) => (
                    <tspan x="560" dy={index ? 17 : 0} key={line}>
                      {line}
                    </tspan>
                  ))}
                </text>
                <text
                  x="560"
                  y="501"
                  textAnchor="middle"
                  className="map-root-subtitle"
                >
                  {centerSubtitle.slice(0, 46)}
                </text>
              </g>
              {children.map((child, index) => {
                const angle =
                  -Math.PI / 2 + (index * Math.PI * 2) / children.length;
                const x = 560 + Math.cos(angle) * 292,
                  y = 400 + Math.sin(angle) * 292;
                const DepartmentIcon = department
                  ? Bot
                  : departments[index].icon;
                const lines = labelLines(child.title);
                return (
                  <g
                    key={child.id}
                    role="button"
                    tabIndex={0}
                    className={`map-node color-${child.color} ${child.active ? "map-node-active" : ""}`}
                    aria-label={
                      department
                        ? `Open ${child.title}`
                        : `Zoom into ${child.title}`
                    }
                    onClick={child.select}
                    onKeyDown={(event) => {
                      if (["Enter", " "].includes(event.key)) {
                        event.preventDefault();
                        child.select();
                      }
                    }}
                  >
                    <circle
                      cx={x}
                      cy={y}
                      r={department ? 46 : 42}
                      className="map-node-ring"
                    />
                    <circle
                      cx={x}
                      cy={y}
                      r={department ? 37 : 34}
                      className="map-node-circle"
                    />
                    <DepartmentIcon
                      x={x - 13}
                      y={y - 13}
                      width="26"
                      height="26"
                      aria-hidden="true"
                    />
                    <text
                      x={x}
                      y={y + 66}
                      textAnchor="middle"
                      className="map-node-title"
                    >
                      {lines.map((line, lineIndex) => (
                        <tspan x={x} dy={lineIndex ? 18 : 0} key={line}>
                          {line}
                        </tspan>
                      ))}
                    </text>
                    <text
                      x={x}
                      y={y + 88 + (lines.length - 1) * 18}
                      textAnchor="middle"
                      className="map-node-subtitle"
                    >
                      {child.active ? "Working" : child.subtitle}
                    </text>
                  </g>
                );
              })}
            </g>
          </svg>
        </div>
      </div>
      <div className="workflow-map-footer">
        <div>
          <strong>
            {department
              ? `Explore ${department.name.toLowerCase()}`
              : "Explore your connected workforce"}
          </strong>
          <p>
            {department
              ? "Select a subagent to inspect its role and run a brief."
              : "Select a department agent to zoom into its subdivisions and subagents."}
          </p>
        </div>
        <div className="map-zoom-controls">
          <Button
            variant="outline"
            size="icon"
            aria-label="Zoom out"
            disabled={zoom <= 0.65}
            onClick={() => setZoom((value) => Math.max(0.65, value - 0.15))}
          >
            <Minus />
          </Button>
          <output aria-label="Zoom level">{Math.round(zoom * 100)}%</output>
          <Button
            variant="outline"
            size="icon"
            aria-label="Zoom in"
            disabled={zoom >= 1.75}
            onClick={() => setZoom((value) => Math.min(1.75, value + 0.15))}
          >
            <Plus />
          </Button>
          <Button
            variant="outline"
            size="icon"
            aria-label="Fit workflow to view"
            onClick={() => setZoom(1)}
          >
            <Maximize />
          </Button>
        </div>
      </div>
      <div className="map-accessible-navigation">
        <label htmlFor="map-department">Navigate to a department</label>
        <select
          id="map-department"
          value={departmentId || "all"}
          onChange={(event) =>
            focus(event.target.value === "all" ? null : event.target.value)
          }
        >
          <option value="all">Entire workforce</option>
          {departments.map((item) => (
            <option key={item.id} value={item.id}>
              {item.name}
            </option>
          ))}
        </select>
      </div>
    </section>
  );
}

export default SystemWorkflow;

