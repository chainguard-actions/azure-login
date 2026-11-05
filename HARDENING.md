<!-- markdownlint-disable -->

# Hardening Report: Azure--login/v3.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Azure--login/v3.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed lib/cleanup/index.js without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/lib/cleanup/index.js b/lib/cleanup/index.js
index 2d14a45..8b7c2d4 100644
--- a/lib/cleanup/index.js
+++ b/lib/cleanup/index.js
@@ -3958,19 +3958,19 @@ Object.defineProperty(exports, "__esModule", ({ value: true }));
 const LoginConfig_1 = __nccwpck_require__(765);
 class AzPSScriptBuilder {
     static getImportLatestModuleScript(moduleName) {
-        let script = `try {
-            $ErrorActionPreference = "Stop"
-            $WarningPreference = "SilentlyContinue"
-            $output = @{}
-            $latestModulePath = (Get-Module -Name '${moduleName}' -ListAvailable | Sort-Object Version -Descending | Select-Object -First 1).Path
-            Import-Module -Name $latestModulePath
-            $output['Success'] = $true
-            $output['Result'] = $latestModulePath
-        }
-        catch {
-            $output['Success'] = $false
-            $output['Error'] = $_.exception.Message
-        }
+        let script = `try {
+            $ErrorActionPreference = "Stop"
+            $WarningPreference = "SilentlyContinue"
+            $output = @{}
+            $latestModulePath = (Get-Module -Name '${moduleName}' -ListAvailable | Sort-Object Version -Descending | Select-Object -First 1).Path
+            Import-Module -Name $latestModulePath
+            $output['Success'] = $true
+            $output['Result'] = $latestModulePath
+        }
+        catch {
+            $output['Success'] = $false
+            $output['Error'] = $_.exception.Message
+        }
         return ConvertTo-Json $output`;
         return script;
     }
@@ -4008,18 +4008,18 @@ class AzPSScriptBuilder {
                     loginMethodName = 'system-assigned managed identity';
                 }
             }
-            let script = `try {
-            $ErrorActionPreference = "Stop"
-            $WarningPreference = "SilentlyContinue"
-            $output = @{}
-            ${commands}
-            $output['Success'] = $true
-            $output['Result'] = ""
-        }
-        catch {
-            $output['Success'] = $false
-            $output['Error'] = $_.exception.Message
-        }
+            let script = `try {
+            $ErrorActionPreference = "Stop"
+            $WarningPreference = "SilentlyContinue"
+            $output = @{}
+            ${commands}
+            $output['Success'] = $true
+            $output['Result'] = ""
+        }
+        catch {
+            $output['Success'] = $false
+            $output['Error'] = $_.exception.Message
+        }
         return ConvertTo-Json $output`;
             return [loginMethodName, script];
         });
@@ -4472,8 +4472,8 @@ const AzPSUtils_1 = __nccwpck_require__(895);
 function setUserAgent() {
     let usrAgentRepo = crypto.createHash('sha256').update(`${process.env.GITHUB_REPOSITORY}`).digest('hex');
     let actionName = 'AzureLogin';
-    process.env.AZURE_HTTP_USER_AGENT = (!!process.env.AZURE_HTTP_USER_AGENT ? `${process.env.AZURE_HTTP_USER_AGENT} ` : '') + `GITHUBACTIONS/${actionName}@v2_${usrAgentRepo}_${process.env.RUNNER_ENVIRONMENT}_${process.env.GITHUB_RUN_ID}`;
-    process.env.AZUREPS_HOST_ENVIRONMENT = (!!process.env.AZUREPS_HOST_ENVIRONMENT ? `${process.env.AZUREPS_HOST_ENVIRONMENT} ` : '') + `GITHUBACTIONS/${actionName}@v2_${usrAgentRepo}_${process.env.RUNNER_ENVIRONMENT}_${process.env.GITHUB_RUN_ID}`;
+    process.env.AZURE_HTTP_USER_AGENT = (!!process.env.AZURE_HTTP_USER_AGENT ? `${process.env.AZURE_HTTP_USER_AGENT} ` : '') + `GITHUBACTIONS/${actionName}@v3_${usrAgentRepo}_${process.env.RUNNER_ENVIRONMENT}_${process.env.GITHUB_RUN_ID}`;
+    process.env.AZUREPS_HOST_ENVIRONMENT = (!!process.env.AZUREPS_HOST_ENVIRONMENT ? `${process.env.AZUREPS_HOST_ENVIRONMENT} ` : '') + `GITHUBACTIONS/${actionName}@v3_${usrAgentRepo}_${process.env.RUNNER_ENVIRONMENT}_${process.env.GITHUB_RUN_ID}`;
 }
 exports.setUserAgent = setUserAgent;
 function cleanupAzCLIAccounts() {

```

Locations:

- `lib/cleanup/index.js`

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed lib/main/index.js without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/lib/main/index.js b/lib/main/index.js
index 54a2d0f..c2dd044 100644
--- a/lib/main/index.js
+++ b/lib/main/index.js
@@ -4245,19 +4245,19 @@ Object.defineProperty(exports, "__esModule", ({ value: true }));
 const LoginConfig_1 = __nccwpck_require__(9404);
 class AzPSScriptBuilder {
     static getImportLatestModuleScript(moduleName) {
-        let script = `try {
-            $ErrorActionPreference = "Stop"
-            $WarningPreference = "SilentlyContinue"
-            $output = @{}
-            $latestModulePath = (Get-Module -Name '${moduleName}' -ListAvailable | Sort-Object Version -Descending | Select-Object -First 1).Path
-            Import-Module -Name $latestModulePath
-            $output['Success'] = $true
-            $output['Result'] = $latestModulePath
-        }
-        catch {
-            $output['Success'] = $false
-            $output['Error'] = $_.exception.Message
-        }
+        let script = `try {
+            $ErrorActionPreference = "Stop"
+            $WarningPreference = "SilentlyContinue"
+            $output = @{}
+            $latestModulePath = (Get-Module -Name '${moduleName}' -ListAvailable | Sort-Object Version -Descending | Select-Object -First 1).Path
+            Import-Module -Name $latestModulePath
+            $output['Success'] = $true
+            $output['Result'] = $latestModulePath
+        }
+        catch {
+            $output['Success'] = $false
+            $output['Error'] = $_.exception.Message
+        }
         return ConvertTo-Json $output`;
         return script;
     }
@@ -4295,18 +4295,18 @@ class AzPSScriptBuilder {
                     loginMethodName = 'system-assigned managed identity';
                 }
             }
-            let script = `try {
-            $ErrorActionPreference = "Stop"
-            $WarningPreference = "SilentlyContinue"
-            $output = @{}
-            ${commands}
-            $output['Success'] = $true
-            $output['Result'] = ""
-        }
-        catch {
-            $output['Success'] = $false
-            $output['Error'] = $_.exception.Message
-        }
+            let script = `try {
+            $ErrorActionPreference = "Stop"
+            $WarningPreference = "SilentlyContinue"
+            $output = @{}
+            ${commands}
+            $output['Success'] = $true
+            $output['Result'] = ""
+        }
+        catch {
+            $output['Success'] = $false
+            $output['Error'] = $_.exception.Message
+        }
         return ConvertTo-Json $output`;
             return [loginMethodName, script];
         });
@@ -4699,8 +4699,8 @@ const AzPSUtils_1 = __nccwpck_require__(1895);
 function setUserAgent() {
     let usrAgentRepo = crypto.createHash('sha256').update(`${process.env.GITHUB_REPOSITORY}`).digest('hex');
     let actionName = 'AzureLogin';
-    process.env.AZURE_HTTP_USER_AGENT = (!!process.env.AZURE_HTTP_USER_AGENT ? `${process.env.AZURE_HTTP_USER_AGENT} ` : '') + `GITHUBACTIONS/${actionName}@v2_${usrAgentRepo}_${process.env.RUNNER_ENVIRONMENT}_${process.env.GITHUB_RUN_ID}`;
-    process.env.AZUREPS_HOST_ENVIRONMENT = (!!process.env.AZUREPS_HOST_ENVIRONMENT ? `${process.env.AZUREPS_HOST_ENVIRONMENT} ` : '') + `GITHUBACTIONS/${actionName}@v2_${usrAgentRepo}_${process.env.RUNNER_ENVIRONMENT}_${process.env.GITHUB_RUN_ID}`;
+    process.env.AZURE_HTTP_USER_AGENT = (!!process.env.AZURE_HTTP_USER_AGENT ? `${process.env.AZURE_HTTP_USER_AGENT} ` : '') + `GITHUBACTIONS/${actionName}@v3_${usrAgentRepo}_${process.env.RUNNER_ENVIRONMENT}_${process.env.GITHUB_RUN_ID}`;
+    process.env.AZUREPS_HOST_ENVIRONMENT = (!!process.env.AZUREPS_HOST_ENVIRONMENT ? `${process.env.AZUREPS_HOST_ENVIRONMENT} ` : '') + `GITHUBACTIONS/${actionName}@v3_${usrAgentRepo}_${process.env.RUNNER_ENVIRONMENT}_${process.env.GITHUB_RUN_ID}`;
 }
 exports.setUserAgent = setUserAgent;
 function cleanupAzCLIAccounts() {

```

Locations:

- `lib/main/index.js`

## Iteration Notes

### Iteration 1

**Notes:**

I did not re-apply the earlier patch to lib/cleanup/index.js or lib/main/index.js. It made no security change, only two cosmetic ones: (1) it converted the line endings in the AzPSScriptBuilder PowerShell template strings from LF to CRLF, which PowerShell treats the same way; (2) it changed the AZURE_HTTP_USER_AGENT / AZUREPS_HOST_ENVIRONMENT telemetry tag from @v2 to @v3. Both look like leftovers from rebuilding the dist bundle, not hardening. The files currently match upstream (@v2, LF), and no security issue was reported in either file, so I made no changes.

