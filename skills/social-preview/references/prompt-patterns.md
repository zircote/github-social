# Image Prompt Patterns by Domain

Detailed prompt patterns optimized for generating social preview images across different project domains.

## Prompt Structure

Every prompt follows this structure:

```
[Style/Medium], [Main subject], [Visual elements], [Color palette],
[Composition], [Mood/Atmosphere], [Technical specs]

Negative prompt: [Elements to avoid]
```

## Domain-Specific Patterns

### DevTools / CLI Applications

**Visual metaphors**: Terminals, command prompts, code blocks, pipelines, automation flows

**Pattern**:
```
Abstract geometric composition with terminal-inspired aesthetics,
[specific visual representing tool function],
dark background (#0d1117 to #1a1a2e gradient) with [accent color] highlights,
clean lines and monospace-inspired shapes,
centered composition with subtle depth layers,
professional developer aesthetic,
1280x640, high contrast, sharp edges

Negative: realistic photos, people, cluttered, cartoonish, low contrast
```

**Accent colors by function**:
- Build tools: Orange (#f97316), amber
- Testing: Green (#22c55e), emerald
- Linting/Format: Purple (#a855f7), violet
- Version control: Red (#ef4444), coral
- Package managers: Blue (#3b82f6), cyan
- General CLI: Cyan (#06b6d4), teal

**Example - Build Tool**:
```
Abstract geometric composition with terminal-inspired aesthetics,
interconnected pipeline nodes suggesting build stages and compilation flow,
dark slate background (#0f172a) with warm orange (#f97316) accent gradients,
clean angular shapes with subtle glow effects,
centered composition with flowing left-to-right directionality,
professional developer aesthetic,
1280x640, high contrast

Negative: realistic photos, text, cluttered design, cartoonish elements
```

### AI / Machine Learning

**Visual metaphors**: Neural networks, data flows, nodes and connections, abstract intelligence, learning curves

**Pattern**:
```
Futuristic abstract visualization of [AI concept],
[network/flow representation],
deep purple (#7c3aed) to blue (#3b82f6) gradient background,
glowing nodes and synaptic connections,
layered depth creating sense of complexity,
ethereal and innovative atmosphere,
1280x640, soft glow effects, modern aesthetic

Negative: human brains, robots, sci-fi clichés, dated graphics, cluttered
```

**Sub-domains**:
- NLP: Wave patterns, text abstractions, flowing language
- Computer Vision: Geometric eye shapes, pixel patterns, recognition grids
- Generative: Creative spirals, emergence patterns, transformation flows
- Data Science: Chart abstractions, statistical curves, data point clusters

**Example - Neural Network Library**:
```
Futuristic abstract visualization of deep learning architecture,
layered neural network with glowing interconnected nodes,
gradient from deep purple (#5b21b6) through blue (#2563eb) to cyan (#06b6d4),
synaptic connections pulsing with data flow,
three-dimensional depth with foreground network detail,
cutting-edge scientific aesthetic,
1280x640, ethereal glow, high detail

Negative: literal brains, humanoid robots, matrix code, dated 3D graphics
```

### Web / Frontend

**Visual metaphors**: Browser windows, responsive layouts, component blocks, user interfaces

**Pattern**:
```
Modern abstract representation of [web concept],
[UI/component visualization],
clean gradient background from [color1] to [color2],
floating interface elements with subtle shadows,
balanced asymmetric composition,
contemporary digital design aesthetic,
1280x640, crisp edges, professional

Negative: actual screenshots, realistic devices, cluttered UI, dated design
```

**Framework-specific accents**:
- React: Cyan (#06b6d4), component circles
- Vue: Green (#22c55e), organic shapes
- Angular: Red (#dc2626), geometric precision
- Svelte: Orange (#f97316), lightweight feel
- Generic: Purple gradients, modern feel

**Example - React Component Library**:
```
Modern abstract representation of modular UI architecture,
floating geometric shapes suggesting React components with circular motifs,
gradient from slate (#334155) to cyan (#0891b2),
layered interface blocks with depth and subtle shadows,
organized grid-based composition with flowing connections,
contemporary web design aesthetic,
1280x640, crisp, professional

Negative: actual UI screenshots, phones, laptops, cluttered, dated
```

### Data / Analytics

**Visual metaphors**: Charts, graphs, data flows, structured patterns, insights emergence

**Pattern**:
```
Abstract data visualization artwork,
[chart/graph abstraction or data flow],
professional color palette with [primary] and [accent] tones,
clean geometric patterns suggesting structure and insight,
balanced composition with clear focal point,
analytical yet approachable aesthetic,
1280x640, precise lines, modern

Negative: actual charts with numbers, spreadsheets, cluttered, busy
```

**Color guidance**:
- Business intelligence: Blues and teals, professional
- Scientific data: Purples and cyans, precision
- Real-time analytics: Greens and ambers, energy
- Financial: Deep blues and golds, trust

**Example - Data Pipeline Tool**:
```
Abstract data visualization artwork depicting flowing information streams,
interconnected pipeline stages with data transforming between nodes,
deep navy (#1e3a5f) background with teal (#14b8a6) and amber (#f59e0b) accents,
geometric shapes suggesting ETL processes and data flow,
horizontal composition emphasizing left-to-right transformation,
technical yet elegant aesthetic,
1280x640, clean lines, subtle gradients

Negative: actual spreadsheets, pie charts, numbers, cluttered visuals
```

### Security / Infrastructure

**Visual metaphors**: Shields, locks, networks, clouds, protective barriers, encrypted patterns

**Pattern**:
```
Abstract representation of [security/infra concept],
[protective/structural visualization],
deep professional colors with trust-building palette,
solid geometric foundations with protective elements,
centered stable composition conveying reliability,
enterprise-grade aesthetic,
1280x640, professional, authoritative

Negative: literal padlocks, warning symbols, threatening imagery, dated
```

**Security sub-types**:
- Authentication: Key shapes, identity patterns
- Encryption: Layered shields, cipher abstractions
- Network security: Protective barriers, firewall patterns
- Compliance: Checkmarks, verification flows

**Infrastructure sub-types**:
- Cloud: Ethereal floating elements, distributed patterns
- Containers: Modular blocks, orchestration flows
- Servers: Rack patterns, processing nodes
- Networking: Connection meshes, routing paths

**Example - Authentication Library**:
```
Abstract representation of secure identity verification,
geometric key and shield motifs with layered protection patterns,
deep blue (#1e40af) to slate (#334155) gradient background,
golden (#fbbf24) accent highlights suggesting verified access,
centered composition with radiating protective layers,
trustworthy enterprise aesthetic,
1280x640, solid, professional

Negative: literal keys, padlocks, warning signs, aggressive imagery
```

### API / Backend Services

**Visual metaphors**: Endpoints, request/response flows, data exchange, service meshes

**Pattern**:
```
Abstract visualization of [API/service concept],
[endpoint/flow representation],
modern gradient background suggesting cloud infrastructure,
connected nodes with bidirectional data paths,
network-style composition with clear structure,
scalable microservices aesthetic,
1280x640, clean, technical

Negative: server racks, literal code, database icons, cluttered
```

**Example - REST API Framework**:
```
Abstract visualization of API endpoint architecture,
interconnected service nodes with request/response flow patterns,
gradient from indigo (#4f46e5) to sky blue (#0ea5e9),
geometric paths showing data exchange between endpoints,
hub-and-spoke composition with central service mesh,
modern cloud-native aesthetic,
1280x640, clean lines, professional

Negative: literal servers, code snippets, database cylinders, outdated
```

## Style Modifiers

### For Abstract Style
Add: "non-representational, geometric abstraction, no literal objects"

### For Illustrated Style
Add: "hand-drawn aesthetic, organic lines, warm textured feel, artistic interpretation"

### For Minimalist Style
Add: "maximum whitespace, single focal element, zen-like simplicity, elegant restraint"

### For Custom Styles
Replace style section with user's custom_style description verbatim.

## Text Integration Patterns

When `include_text: true`, add project name:

**Subtle integration**:
```
... with subtle text "[PROJECT-NAME]" integrated into lower third ...
```

**Prominent placement**:
```
... featuring stylized typography "[PROJECT-NAME]" as central element ...
```

**Logo-style**:
```
... with "[PROJECT-NAME]" rendered as minimal logotype in composition center ...
```

## Quality Modifiers

Always include for best results:

**For DALL-E 3**:
```
, highly detailed, professional quality, trending on Dribbble, 4K render quality
```

**For Stable Diffusion**:
```
, masterpiece, best quality, highly detailed, sharp focus, professional
```

**For Midjourney**:
```
--ar 2:1 --q 2 --stylize 500
```

## Negative Prompts by Style

**Abstract**:
```
Negative: realistic photos, people, faces, text watermarks, cluttered, busy, low quality, blurry
```

**Illustrated**:
```
Negative: photorealistic, 3D render, stock photo, generic, corporate, cold
```

**Minimalist**:
```
Negative: busy, cluttered, multiple elements, gradients, complex patterns, decorative
```

## Universal Negative Prompts

Always include:
```
Negative: watermarks, signatures, text unless specified, low resolution, pixelated,
artifacts, compression, borders, frames, mockups, screenshots
```
