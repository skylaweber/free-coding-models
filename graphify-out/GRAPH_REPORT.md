# Graph Report - src  (2026-05-04)

## Corpus Check
- 44 files · ~98,716 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 552 nodes · 1282 edges · 20 communities detected
- Extraction: 90% EXTRACTED · 10% INFERRED · 0% AMBIGUOUS · INFERRED: 125 edges (avg confidence: 0.8)
- Token cost: 0 input · 0 output

## Community Hubs (Navigation)
- [[_COMMUNITY_FileRouter Utils|File/Router Utils]]
- [[_COMMUNITY_UI Rendering Helpers|UI Rendering Helpers]]
- [[_COMMUNITY_App Lifecycle & Cache|App Lifecycle & Cache]]
- [[_COMMUNITY_Config Management|Config Management]]
- [[_COMMUNITY_Tool Bootstrap & Install|Tool Bootstrap & Install]]
- [[_COMMUNITY_Analysis & Model Filtering|Analysis & Model Filtering]]
- [[_COMMUNITY_Install & Catalog|Install & Catalog]]
- [[_COMMUNITY_Cleanup Functions|Cleanup Functions]]
- [[_COMMUNITY_OpenClaw Integration|OpenClaw Integration]]
- [[_COMMUNITY_Ping & Quota Monitoring|Ping & Quota Monitoring]]
- [[_COMMUNITY_Model Management|Model Management]]
- [[_COMMUNITY_Theme & Colors|Theme & Colors]]
- [[_COMMUNITY_Telemetry|Telemetry]]
- [[_COMMUNITY_Test Framework|Test Framework]]
- [[_COMMUNITY_Config Security|Config Security]]
- [[_COMMUNITY_Kilo Config|Kilo Config]]
- [[_COMMUNITY_Provider Quota Fetchers|Provider Quota Fetchers]]
- [[_COMMUNITY_Command Palette|Command Palette]]
- [[_COMMUNITY_Model Merger|Model Merger]]
- [[_COMMUNITY_Changelog Loader|Changelog Loader]]

## God Nodes (most connected - your core abstractions)
1. `RouterRuntime` - 38 edges
2. `runApp()` - 29 edges
3. `installProviderEndpoints()` - 23 edges
4. `prepareExternalToolLaunch()` - 20 edges
5. `saveConfig()` - 19 edges
6. `renderTable()` - 18 edges
7. `cleanupLegacyProxyArtifacts()` - 17 edges
8. `renderRouterDashboard()` - 16 edges
9. `isPlainObject()` - 15 edges
10. `normalizeRouterConfig()` - 14 edges

## Surprising Connections (you probably didn't know these)
- `renderTable()` --calls--> `calculateViewport()`  [INFERRED]
  /Users/vava/Documents/GitHub/free-coding-models/src/render-table.js → /Users/vava/Documents/GitHub/free-coding-models/src/render-helpers.js
- `renderTable()` --calls--> `getTheme()`  [INFERRED]
  /Users/vava/Documents/GitHub/free-coding-models/src/render-table.js → /Users/vava/Documents/GitHub/free-coding-models/src/theme.js
- `runApp()` --calls--> `detectActiveTheme()`  [INFERRED]
  /Users/vava/Documents/GitHub/free-coding-models/src/app.js → /Users/vava/Documents/GitHub/free-coding-models/src/theme.js
- `runApp()` --calls--> `fetchOpenRouterFreeModels()`  [INFERRED]
  /Users/vava/Documents/GitHub/free-coding-models/src/app.js → /Users/vava/Documents/GitHub/free-coding-models/src/analysis.js
- `runApp()` --calls--> `formatResultsAsJSON()`  [INFERRED]
  /Users/vava/Documents/GitHub/free-coding-models/src/app.js → /Users/vava/Documents/GitHub/free-coding-models/src/utils.js

## Communities

### Community 0 - "File/Router Utils"
Cohesion: 0.06
Nodes (42): atomicWriteJson(), attachClientAbort(), buildDefaultRouterSet(), buildProviderModelsUrl(), buildRouterSetFromFavorites(), buildUpstreamMeta(), cloneHeadersForUpstream(), ensureRouterConfigForDaemon() (+34 more)

### Community 1 - "UI Rendering Helpers"
Cohesion: 0.08
Nodes (56): calculateViewport(), clampOverlayOffset(), displayWidth(), keepOverlayTargetVisible(), padEndDisplay(), sliceOverlayLines(), stripAnsi(), tintOverlayLines() (+48 more)

### Community 2 - "App Lifecycle & Cache"
Cohesion: 0.06
Nodes (34): runApp(), clearCache(), getCachePath(), isCacheFresh(), loadCache(), saveCache(), createKeyHandler(), createMouseEventHandler() (+26 more)

### Community 3 - "Config Management"
Cohesion: 0.11
Nodes (40): buildPersistedConfig(), cloneConfigValue(), createBackup(), _emptyConfig(), _emptyProfileSettings(), isPlainObject(), listApiKeys(), loadConfig() (+32 more)

### Community 4 - "Tool Bootstrap & Install"
Cohesion: 0.12
Nodes (39): executableSuffixes(), getToolBootstrapMeta(), getToolInstallPlan(), isRunnableFile(), isToolInstalled(), pathEntries(), resolveBinaryPath(), resolveToolBinaryPath() (+31 more)

### Community 5 - "Analysis & Model Filtering"
Cohesion: 0.08
Nodes (27): fetchOpenRouterFreeModels(), filterByTierOrExit(), runFiableMode(), buildCliHelpLines(), buildCliHelpText(), formatEntry(), paint(), renderTable() (+19 more)

### Community 6 - "Install & Catalog"
Cohesion: 0.16
Nodes (31): backupIfExists(), buildInstallRecord(), canonicalizeToolMode(), ensureDirFor(), getDefaultPaths(), getDirectInstallSupport(), getManagedProviderId(), getManagedProviderLabel() (+23 more)

### Community 7 - "Cleanup Functions"
Cohesion: 0.22
Nodes (25): cleanupAider(), cleanupAmp(), cleanupCrush(), cleanupGoose(), cleanupLegacyEnvFiles(), cleanupLegacyProxyArtifacts(), cleanupMainConfig(), cleanupOpenClaw() (+17 more)

### Community 8 - "OpenClaw Integration"
Cohesion: 0.18
Nodes (18): getApiKey(), getOpenClawConfigPath(), loadOpenClawConfig(), saveOpenClawConfig(), spawnOpenClawCli(), startOpenClaw(), buildOpenAiCompatibleProviderConfig(), loadOpenCodeConfig() (+10 more)

### Community 9 - "Ping & Quota Monitoring"
Cohesion: 0.15
Nodes (19): buildPingRequest(), extractQuotaPercent(), fetchProviderQuotaPercent(), getHeaderValue(), getProviderQuotaPercentCached(), ping(), resolveCloudflareUrl(), usagePlaceholderForProvider() (+11 more)

### Community 10 - "Model Management"
Cohesion: 0.19
Nodes (13): loadBackups(), parseAiderConfig(), parseAmpConfig(), parseCrushConfig(), parseGooseConfig(), parseKiloConfig(), parseOpenHandsConfig(), parsePiConfig() (+5 more)

### Community 11 - "Theme & Colors"
Cohesion: 0.23
Nodes (11): buildStyle(), currentPalette(), detectActiveTheme(), getProviderRgb(), getReadableTextRgb(), getTheme(), getThemeStatusLabel(), getTierRgb() (+3 more)

### Community 12 - "Telemetry"
Cohesion: 0.33
Nodes (11): buildTelemetryProperties(), ensureTelemetryConfig(), getTelemetryDistinctId(), getTelemetrySystem(), getTelemetryTerminal(), isTelemetryDebugEnabled(), isTelemetryEnabled(), parseTelemetryEnv() (+3 more)

### Community 13 - "Test Framework"
Cohesion: 0.22
Nodes (4): classifyToolTranscript(), detectTranscriptFindings(), normalizeTestfcmToolName(), resolveTestfcmToolSpec()

### Community 14 - "Config Security"
Cohesion: 0.47
Nodes (8): checkConfigSecurity(), fixConfigPermissions(), formatMode(), formatModeRwx(), getConfigPath(), getConfigPermissions(), isConfigSecure(), promptSecurityFix()

### Community 15 - "Kilo Config"
Cohesion: 0.36
Nodes (7): buildOpenAiCompatibleProviderConfig(), getKiloConfigPath(), loadKiloConfig(), saveKiloConfig(), getKiloModelId(), spawnKilo(), startKilo()

### Community 16 - "Provider Quota Fetchers"
Cohesion: 0.36
Nodes (7): createProviderQuotaFetcher(), fetchOpenRouterRaw(), fetchProviderQuota(), fetchSiliconFlowRaw(), makeCacheKey(), parseOpenRouterResponse(), parseSiliconFlowResponse()

### Community 17 - "Command Palette"
Cohesion: 0.53
Nodes (5): buildCommandPaletteEntries(), buildCommandPaletteTree(), filterCommandPaletteEntries(), flattenCommandTree(), fuzzyMatchCommand()

### Community 18 - "Model Merger"
Cohesion: 0.6
Nodes (3): buildMergedModels(), parseCtxK(), parseSwePercent()

### Community 19 - "Changelog Loader"
Cohesion: 0.83
Nodes (3): formatChangelogForDisplay(), getLatestChanges(), loadChangelog()

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `runApp()` connect `App Lifecycle & Cache` to `File/Router Utils`, `Config Management`, `Analysis & Model Filtering`, `Install & Catalog`, `Theme & Colors`, `Config Security`?**
  _High betweenness centrality (0.225) - this node is a cross-community bridge._
- **Why does `getApiKey()` connect `OpenClaw Integration` to `File/Router Utils`, `Config Management`, `Tool Bootstrap & Install`, `Install & Catalog`, `Kilo Config`?**
  _High betweenness centrality (0.135) - this node is a cross-community bridge._
- **Why does `renderTable()` connect `Analysis & Model Filtering` to `File/Router Utils`, `UI Rendering Helpers`, `App Lifecycle & Cache`, `Theme & Colors`?**
  _High betweenness centrality (0.127) - this node is a cross-community bridge._
- **Are the 28 inferred relationships involving `runApp()` (e.g. with `detectActiveTheme()` and `checkConfigSecurity()`) actually correct?**
  _`runApp()` has 28 INFERRED edges - model-reasoned connections that need verification._
- **Are the 3 inferred relationships involving `installProviderEndpoints()` (e.g. with `saveConfig()` and `getToolMeta()`) actually correct?**
  _`installProviderEndpoints()` has 3 INFERRED edges - model-reasoned connections that need verification._
- **Are the 11 inferred relationships involving `saveConfig()` (e.g. with `getTelemetryDistinctId()` and `.saveRouterConfig()`) actually correct?**
  _`saveConfig()` has 11 INFERRED edges - model-reasoned connections that need verification._
- **Should `File/Router Utils` be split into smaller, more focused modules?**
  _Cohesion score 0.06 - nodes in this community are weakly interconnected._