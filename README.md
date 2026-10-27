# WearSight

Turn walkaround footage into inspection evidence.

WearSight is an AI-assisted walkaround inspection project for equipment operators, trainee inspectors, and technicians. It is being designed to turn short videos of suspected visible defects into evidence-backed draft maintenance work orders, with plain-language explanations for learning and human review.

Status: Planning and feasibility testing. The workflow and features below are planned; a complete application and validated performance results are not yet available.

## Why WearSight?

During a walkaround inspection, noticing a possible problem is only one step. The operator must also capture useful evidence, describe what they see, and explain why someone should investigate it. Trainee inspectors may need additional help understanding the finding.

WearSight aims to support that process, starting with mining and workshop settings in Tarkwa, Ghana.

## Intended users

Trainee inspectors learning how to document visible equipment defects.

Equipment operators recording findings during walkaround checks.

Technicians reviewing inspection evidence and draft maintenance actions.

## Planned workflow

Record or select a short inspection video, initially around 10–20 seconds.

Mark a suspected defect with a bounding box on an initial frame.

Run the SAM 2.1 tracking notebook in a GPU-enabled Google Colab session.

Export and manually download the run folder containing frames, overlays, metadata, and mask metrics.

Load the exported run into the local Streamlit app.

Review video-quality warnings and selected evidence frames.

Request a structured draft work order from a vision-language model.

Review the explanation, evidence, suggested actions, and any human-review flag; download the validated JSON report.

The Colab tracking step is manual in the initial prototype. The first Streamlit version will load exported run folders; it is not a live video-processing service. The user identifies the region to inspect. Segmentation follows that prompted region and does not by itself establish the type or seriousness of a defect.

## Initial defect scope

Oil leaks or visible oil staining.

Rust or visible corrosion.

Frayed or worn edges, such as a belt or rope.

The feasibility test will determine which defect types are reliable enough for the prototype. If tracking is inadequate, the fallback is vision-language analysis of evenly sampled frames, with the absence of masks disclosed.

## Planned features

Prompted tracking of a suspected defect across video frames.

Keyframe selection using mask metrics, with uniform sampling as a baseline.

Original frames and mask overlays displayed side by side.

JSON work orders validated with Pydantic.

Plain-language explanations for trainee inspectors.

Explicit flags for findings requiring human review.

Clear handling of poor-quality inputs and model-service failures.

Downloadable JSON reports and documented evaluation results.

Technology plan

## Component

Planned technology and role

Local runtime

Python 3.11

Interface

Streamlit

Segmentation and tracking

SAM 2.1 in GPU-enabled Colab

Vision-language analysis

Gemini through the Google Gen AI SDK; model selected through configuration

Output validation

Pydantic

Video and image utilities

OpenCV, NumPy, Pillow

Evaluation and tests

pandas and pytest

The local app and pipeline will consume exported tracking results. They will not require local SAM 2.1 or PyTorch imports.

## Planned repository layout
Path
Purpose
app/
Streamlit interface
pipeline/
Run loading, keyframes, model calls, validation, and review rules
notebooks/
Colab tracking and export notebooks
eval/
Evaluation scripts and experiment metrics
tests/
Automated tests and small fixtures
data/
Clip labels, source records, and permitted sample data
docs/
Evidence notes, limitations, and evaluation results
runs/
Generated run folders; ignored by Git
.env.example
Environment-variable template with no credentials
DEV_LOG.md
Development decisions and reviewed AI-assisted contributions
AGENTS.md
Coding-agent instructions
Setup status
Runnable setup instructions will be added when the scaffold and first working slice are available. Planned requirements are Python 3.11, a local virtual environment, a GPU-enabled Colab session for tracking, and a Google AI Studio API key for live model analysis.

Local secrets will be kept in an ignored .env file. Model names will be read from configuration. API access and hosted GPU availability depend on provider quotas; any usage limits and costs observed during development will be documented.

## Evaluation plan

WearSight will compare mask-guided keyframes with evenly sampled frames, then compare image-only analysis with analysis that also receives mask metrics and explicitly simulated telemetry.

Planned measures include defect-type correctness, severity agreement within one point on a five-point scale, human-review flag rate, and time to produce a report compared with a small manual baseline. Results will include sample sizes, difficult cases, and failures. No accuracy or time-saving claims are made yet.

## Limits and human review

WearSight is a prototype inspection aid. Findings and suggested actions require human review before maintenance or equipment-use decisions.

The initial scope covers visible surface evidence in recorded clips. It does not establish internal equipment condition, certify equipment as safe, or predict future failures. The initial severity score is a model estimate that requires evaluation and review.

Some test defects may be staged. Any telemetry included in the prototype is simulated, labeled as simulated, and is not a measurement from the inspected equipment. Poor lighting, blur, small defects, camera motion, and occlusion may reduce the quality of results. A missing finding does not establish that equipment is defect-free.

## Data and attribution

Use only self-recorded clips or clips with licenses permitting the intended use. Record each clip's source and license in data/LICENSES.csv. Keep private footage and identifying information out of the public repository unless the relevant permissions are in place.

Third-party models, libraries, checkpoints, and datasets retain their respective licenses. Record those licenses separately from the project's original code.

## Development and contribution

Use Issues to report a problem or suggest a focused improvement. When reporting a failure, include the expected behavior, observed behavior, and a reproducible example that contains no private data or credentials.

Meaningful AI-assisted development contributions will be reviewed, tested, and recorded in DEV_LOG.md. Preparation work and work completed during the Global AI Builder's Challenge will be documented separately. A pre-event tag is planned to capture the starting point before event implementation begins.

## License

See LICENSE for the license selected by the repository owner for WearSight's original code. Third-party components and media retain their own license terms.
