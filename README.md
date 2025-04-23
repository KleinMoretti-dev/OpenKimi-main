March 2025 (version 1.99)

更新后显示发行说明

Update 1.99.1: The update addresses these security issues.

Update 1.99.2: The update addresses these issues.

Update 1.99.3: The update addresses these issues.

Welcome to the March 2025 release of Visual Studio Code. There are many updates in this version that we hope you'll like, some of the key highlights include:

Agent mode

Agent mode is available in VS Code Stable. Enable it by setting chat.agent.enabled (more...).
Extend agent mode with Model Context Protocol (MCP) server tools (more...).
Try the new built-in tools in agent mode for fetching web content, finding symbol references, and deep thinking (more...).
Code editing

Next Edit Suggestions is now generally available (more...).
Benefit from fewer distractions such as diagnostics events while AI edits are applied in the editor (more...).
Chat

Use your own API keys to access more language models in chat (preview) (more...).
Easily switch between ask, edit, and agent mode from the unified chat experience (more...).
Experience improved workspace search speed and accuracy with instant remote workspace indexing (more...).
Notebook editing

Create and edit notebooks as easily as code files with support for edit and agent mode (more...).
If you'd like to read these release notes online, go to Updates on code.visualstudio.com. Insiders: Want to try new features as soon as possible? You can download the nightly Insiders build and try the latest updates as soon as they are available.

Chat
Agent mode is available in VS Code Stable
Setting: chat.agent.enabled

We're happy to announce that agent mode is available in VS Code Stable! Enable it by setting chat.agent.enabled. If you do not see the setting, make sure to reload VS Code. Enabling the setting will no longer be needed in the following weeks, as we roll out enablement by default to all users.

Check out the agent mode documentation or select agent mode from the chat mode picker in the Chat view.

Screenshot that shows the Chat view, highlighting agent mode selected in the chat mode picker.

Model Context Protocol server support
This release supports Model Context Protocol (MCP) servers in agent mode. MCP offers a standardized method for AI models to discover and interact with external tools, applications, and data sources. When you input a chat prompt using agent mode in VS Code, the model can invoke various tools to perform tasks such as file operations, accessing databases, or retrieving web data. This integration enables more dynamic and context-aware coding assistance.

MCP servers can be configured under the mcp section in your user, remote, or .code-workspace settings, or in .vscode/mcp.json in your workspace. The configuration supports input variables to avoid hard-coding secrets and constants. For example, you can use ${env:API_KEY} to reference an environment variable or ${input:ENDPOINT} to prompt for a value when the server is started.

You can use the MCP: Add Server command to quickly set up an MCP server from a command line invocation, or use an AI-assisted setup from an MCP server published to Docker, npm, or PyPI.

When a new MCP server is added, a refresh action is shown in the Chat view, which can be used to start the server and discover the tools. Afterwards, servers are started on-demand to save resources.

 Theme: Codesong (preview on vscode.dev)

If you've already been using MCP servers in other applications such as Claude Desktop, VS Code will discover them and offer to run them for you. This behavior can be toggled with the setting   chat.mcp.discovery.enabled .

You can see the list of MCP servers and their current status using the MCP: List Servers command, and pick the tools available for use in chat by using the Select Tools button in agent mode.

You can read more about how to install and use MCP servers in our documentation.

Agent mode tools
This milestone, we have added several new built-in tools to agent mode.

Thinking tool (Experimental)
Setting: github.copilot.chat.agent.thinkingTool.

Inspired by Anthropic's research, we've added support for a thinking tool in agent mode that can be used to give any model the opportunity to think between tool calls. This improves our agent's performance on complex tasks in-product and on the SWE-bench eval.

Fetch tool
Use the #fetch tool for including content from a publicly accessible webpage in your prompt. For instance, if you wanted to include the latest documentation on a topic like MCP, you can ask to fetch the full documentation (which is conveniently ready for an LLM to consume) and use that in a prompt. Here's a video of what that might look like:


In agent mode, this tool is picked up automatically but you can also reference it explicitly in the other modes via #fetch, along with the URL you are looking to fetch.

This tool works by rendering the webpage in a headless browser window in which the data of that page is cached locally, so you can freely ask the model to fetch the contents over and over again without the overhead of re-rendering.

Let us know how you use the #fetch tool, and what features you'd like to see from it!

Fetch tool limitations:

Currently, JavaScript is disabled in this browser window. The tool will not be able to acquire much context if the website depends entirely on JavaScript to render content. This is a limitation we are considering changing and likely will change to allow JavaScript.
Due to the headless nature, we are unable to fetch pages that are behind authentication, as this headless browser exists in a different browser context than the browser you use. Instead, consider using MCP to bring in an MCP server that is purpose-built for that target, or a generic browser MCP server such as the Playwright MCP server.
Usages tool
The #usages tool is a combination of "Find All References", "Find Implementation", and "Go to Definition". This tool can help chat to learn more about a function, class, or interface. For instance, chat can use this tool to look for sample implementations of an interface or to find all places that need to be changed when making a refactoring.

In agent mode this tool will be picked up automatically but you can also reference it explicitly via #usages

Create a new workspace with agent mode (Experimental)
Setting: github.copilot.chat.newWorkspaceCreation.enabled

You can now scaffold a new VS Code workspace in agent mode. Whether you’re setting up a VS Code extension, an MCP server, or other development environments, agent mode helps you to initialize, configure, and launch these projects with the necessary dependencies and settings.


VS Code extension tools in agent mode
Several months ago, we finalized our extension API for language model tools contributed by VS Code extensions. Now, you can use these tools in agent mode.

Any tool contributed to this API which sets toolReferenceName and canBeReferencedInPrompt in its configuration is automatically available in agent mode.

By contributing a tool in an extension, it has access to the full VS Code extension APIs, and can be easily installed via the Extension Marketplace.

Similar to tools from MCP servers, you can enable and disable these with the Select Tools button in agent mode. See our language model tools extension guide to build your own!

Agent mode tool approvals
As part of completing the tasks for a user prompt, agent mode can run tools and terminal commands. This is powerful but potentially comes with risks. Therefore, you need to approve the use of tools and terminal commands in agent mode.

To optimize this experience, you can now remember that approval on a session, workspace, or application level. This is not currently enabled for the terminal tool, but we plan to develop an approval system for the terminal in future releases.

Screenshot that shows the agent mode tool Continue button dropdown options for remembering approval.

In case you want to auto-approve all tools, you can now use the experimental   chat.tools.autoApprove setting. This will auto-approve all tools, and VS Code will not ask for confirmation when a language model wishes to run tools. Bear in mind that with this setting enabled, you will not have the opportunity to cancel potentially destructive actions a model wants to take.

We plan to expand this setting with more granular capabilities in the future.

Agent evaluation on SWE-bench
VS Code's agent achieves a pass rate of 56.0% on swebench-verified with Claude 3.7 Sonnet, following Anthropic's research on configuring agents to execute without user input in the SWE-bench environment. Our experiments have translated into shipping improved prompts, tool descriptions and tool design for agent mode, including new tools for file edits that are in-distribution for Claude 3.5 and 3.7 Sonnet models.

Unified Chat view
For the past several months, we've had a "Chat" view for asking questions to the language model, and a "Copilot Edits" view for an AI-powered code editing session. This month, we aim to streamline the chat-based experience by merging the two views into one Chat view. In the Chat view, you'll see a dropdown with three modes:

Screenshot that shows the chat mode picker in the Chat view.

Ask: This is the same as the previous Chat view. Ask questions about your workspace or coding in general, using any model. Use @ to invoke built-in chat participants or from installed extensions. Use # to attach any kind of context manually.
Agent: Start an agentic coding flow with a set of tools that let it autonomously collect context, run terminal commands, or take other actions to complete a task. Agent mode is enabled for all VS Code Insiders users, and we are rolling it out to more and more users in VS Code Stable.
Edit: In Edit mode, the model can make directed edits to multiple files. Attach #codebase to let it find the files to edit automatically. But it won't run terminal commands or do anything else automatically.
Note: If you don't see agent mode in this list, then either it has not yet been enabled for you, or it's disabled by organization policy and needs to be enabled by the organization owner.

Besides making your chat experience simpler, this unification enables a few new features for AI-powered code editing:

Switch modes in the middle of a conversation: For example, you might start brainstorming an app idea in ask mode, then switch to agent mode to execute the plan. Tip: press Ctrl+. to change modes quickly.
Edit sessions in history: Use the Show Chats command (clock icon at the top of the Chat view) to restore past edit sessions and keep working on them.
Move chat to editor or window: Select Open Chat in New Editor/New Window to pop out your chat conversation from the side bar into a new editor tab or separate VS Code window. Chat has supported this for a long time, but now you can run your edit/agent sessions from an editor pane or a separate window as well.
Multiple agent sessions: Following from the above point, this means that you can even run multiple agent sessions at the same time. You might like to have one chat in agent mode working on implementing a feature, and another independent session for doing research and using other tools. Directing two agent sessions to edit files at the same time is not recommended, it can lead to confusion.
Bring Your Own Key (BYOK) (Preview)
Copilot Pro and Copilot Free users can now bring their own API keys for popular providers such as Azure, Anthropic, Gemini, Open AI, Ollama, and Open Router. This allows you to use new models that are not natively supported by Copilot the very first day that they're released.

To try it, select Manage Models... from the model picker. We’re actively exploring support for Copilot Business and Enterprise customers and will share updates in future releases. To learn more about this feature, head over to our docs.

A screenshot of a "Manage Models - Preview" dropdown menu in a user interface. The dropdown has the label "Select a provider" at the top, with a list of options below it. The options include "Anthropic" (highlighted in blue), "Azure," "Gemini," "OpenAI," "Ollama," and "OpenRouter." A gear icon is displayed next to the "Anthropic" option.

Reusable prompt files
Improved configuration
Setting:   chat.promptFilesLocations

The   chat.promptFilesLocations setting now supports glob patterns in file paths. For example, to include all .prompt.md files in the currently open workspace, you can set the path to { "**": true }.

Additionally, the configuration now respects case sensitivity on filesystems where it applies, aligning with the behavior of the host operating system.

Improved prompt file editing
Your .prompt.md files now offer basic autocompletion for filesystem paths and highlight valid file references. Broken links on the other hand now appear as warning or error squiggles and provide detailed diagnostic information.
You can now manage prompts using edit and delete actions in the prompt file list within the Chat: Use Prompt command.
Folder references in prompt files are no longer flagged as invalid.
Markdown comments are now properly handled, for instance, all commented-out links are ignored when generating the final prompt sent to the LLM model.
Alignment with custom instructions
The .github/copilot-instructions.md file now behaves like any other reusable .prompt.md file, with support for nested link resolution and enhanced language features. Furthermore, any .prompt.md file can now be referenced and is handled appropriately.

Learn more about custom instructions.

User prompts
The Create User Prompt command now allows creating a new type of prompts called user prompts. These are stored in the user data folder and can be synchronized across machines, similar to code snippets or user settings. The synchronization can be configured in Sync Settings by using the Prompts item in the synchronization resources list.

Improved vision support (Preview)
Last iteration, Copilot Vision was enabled for GPT-4o. Check our release notes to learn more about how you can attach and use images in chat.

This release, you can attach images from any browser via drag and drop. Images drag and dropped from browsers must have the correct url extension, with .jpg, .png, .gif, .webp, or .bmp.


Configure the editor
Unified chat experience
We have streamlined the chat experience in VS Code into a single unified Chat view. Instead of having to move between separate views and lose the context of a conversation, you can now easily switch between the different chat modes.

Screenshot that shows the chat mode picker in the Chat view.

Depending on your scenario, use either of these modes, and freely move mid-conversation:

Ask mode: optimized for asking questions about your codebase and brainstorming ideas.
Edit mode: optimized for making edits across multiple files in your codebase.
Agent mode: optimized for an autonomous coding flow, combining code edits and tool invocations.
Get more details about the unified chat view.

Faster workspace searches with instant indexing
Remote workspace indexes accelerate searching large codebases for relevant code snippets that AI uses while answering questions and generating edits. These remote indexes are especially useful for large codebases with tens or even hundreds of thousands of files.

Previously, you'd have to press a button or run a command to build and start using a remote workspace index. With our new instant indexing support, we now automatically build the remote workspace index when you first try to ask a #codebase/@workspace question. In most cases, this remote index can be built in a few seconds. Once built, any codebase searches that you or anyone else working with that repo in VS Code makes will automatically use the remote index.

Keep in mind that remote workspaces indexes are currently only available for code stored on GitHub. To use a remote workspace index, make sure your workspace contains a git project with a GitHub remote. You can use the Copilot status menu to see the type of index currently being used:

Screenshot that shows the workspace index status in the Copilot Status Bar menu.

To manage load, we are slowly rolling out instant indexing over the next few weeks, so you may not see it right away. You can still run the GitHub Copilot: Build remote index command command to start using a remote index when instant indexing is not yet enabled for you.

Copilot status menu
The Copilot status menu, accessible from the Status Bar, is now enabled for all users. This milestone we added some new features to it:

View workspace index status information at any time.

Screenshot that shows the workspace index status of a workspace in the Copilot menu.

View if code completions are enabled for the active editor.

A new icon reflects the status, so that you can quickly see if code completions are enabled or not.

Screenshot that shows the Copilot status icon when completions is disabled.

Enable or disable code completions and NES.

Out of the box Copilot setup (Experimental)
Setting:   chat.setupFromDialog

We are shipping an experimental feature to show functional chat experiences out of the box. This includes the Chat view, editor/terminal inline chat, and quick chat. The first time you send a chat request, we will guide you through signing in and signing up for Copilot Free.


If you want to see this experience for yourself, enable the   chat.setupFromDialog setting.

Chat prerelease channel mismatch
If you have the prerelease version of the Copilot Chat extension installed in VS Code Stable, a new welcome screen will inform you that this configuration is not supported. Due to rapid development of chat features, the extension will not activate in VS Code Stable.

The welcome screen provides options to either switch to the release version of the extension or download VS Code Insiders.

Screenshot that shows the welcome view of chat, indicating that the pre-release version of the extension is not supported in VS Code stable. A button is shown to switch to the release version, and a secondary link is shown to switch to VS Code Insiders.

Semantic text search improvements (Experimental)
Setting: github.copilot.chat.search.semanticTextResults

AI-powered semantic text search is now enabled by default in the Search view. Use the Ctrl+I keyboard shortcut to trigger a semantic search, which shows you the most relevant results based on your query, on top of the regular search results.


You can also reference the semantic search results in your chat prompt by using the #searchResults tool. This allows you to ask the LLM to summarize or explain the results, or even generate code based on them.


Settings editor search updates
By default, the Settings editor search now uses the key-matching algorithm we introduced in the previous release. It also shows additional settings even when the settings ID matches exactly with a known setting.


Theme: Light Pink (preview on vscode.dev)

New setting for window controls (Linux, Windows)
Setting:   window.controlsStyle

If you have set the title bar style (  window.titleBarStyle ) to custom, you can now choose between three different styles for the window controls.

native: this is the default and renders window controls according to the underlying platform
custom: renders window controls with custom styling if you prefer that over the native one
hidden: hides window controls entirely if you want to gain some space in the title bar and are a more keyboard-centric user
Code Editing
Next Edit Suggestions general availability
Setting: github.copilot.nextEditSuggestions.enabled

We're happy to announce the general availability of Next Edit Suggestions (NES)! In addition, we've also made several improvements to the overall user experience of NES:

Make edit suggestions more compact, less interfering with surrounding code, and easier to read at a glance.
Updates to the gutter indicator to make sure that all suggestions are more easily noticeable.

AI edits improvements
We have done some smaller tweaks when generating edits with AI:

Mute diagnostics events outside the editor while rewriting a file with AI edits. Previously, we already disabled squiggles in this scenario. These changes reduce flicker in the Problems panel and also ensure that we don't issue requests for the quick fix code actions.

We now explicitly save a file when you decide to keep the AI edits.

Tool-based edit mode
Setting:   chat.edits2.enabled

We're making a change to the way edit mode in chat operates. The new edit mode uses the same approach as agent mode, where it lets the model call a tool to make edits to files. An upside to this alignment is that it enables you to switch seamlessly between all three modes, while providing a huge simplification to how these modes work under the hood.

A downside is that this means that the new mode only works with the same reduced set of models that agent mode works with, namely models that support tool calling and have been tested to be sure that we can have a good experience when tools are involved. You may notice models like o3-mini and Claude 3.7 (Thinking) missing from the list in edit mode. If you'd like to keep using those models for editing, disable the   chat.edits2.enabled setting to revert to the previous edit mode. You'll be asked to clear the session when switching modes.

We've learned that prompting to get consistent results across different models is harder when using tools, but we are working on getting these models lit up for edit (and agent) modes.

This setting will be enabled gradually for users in VS Code Stable.

Inline suggestion syntax highlighting
Setting:   editor.inlineSuggest.syntaxHighlightingEnabled

With this update, syntax highlighting for inline suggestions is now enabled by default. Notice in the following screenshot that the code suggestion has syntax coloring applied to it.

Screenshot of the editor, showing that syntax highlighting is enabled for ghost text.

If you prefer inline suggestions without syntax highlighting, you can disable it with   editor.inlineSuggest.syntaxHighlightingEnabled .

Screenshot of the editor showing that highlighting for ghost text is turned off.

Tree-Sitter based syntax highlighting (Preview)
Setting:   editor.experimental.preferTreeSitter.css and   editor.experimental.preferTreeSitter.regex

Building upon the previous work for using Tree-Sitter for syntax highlighting, we now support experimental, Tree-Sitter based, syntax highlighting for CSS files and for regular expressions within TypeScript.

Notebooks
Minimal version of Jupyter notebook document to 4.5
The default version of nbformat for new notebooks has been bumped from 4.2 to 4.5, which will now set id fields for each cell of the notebook to help with calculating diffs. You can also manually update existing notebooks by setting the nbformat_minor to 5 in the raw JSON of the notebook.

AI notebook editing improvements
AI-powered editing support for notebooks (including agent mode) is now available in the Stable release. This was added last month as a preview feature in VS Code Insiders.

You can now use chat to edit notebook files with the same intuitive experience as editing code files: modify content across multiple cells, insert and delete cells, and change cell types. This feature provides a seamless workflow when working with data science or documentation notebooks.

New notebook tool
VS Code now provides a dedicated tool for creating new Jupyter notebooks directly from chat. This tool plans and creates a new notebook based on your query.

Use the new notebook tool in agent mode or edit mode (make sure to enable the improved edit mode with   chat.edits2.enabled ). If you're using ask mode, type /newNotebook in the chat prompt to create a new notebook.


Navigate through AI edits
Use the diff toolbars to iterate through and review each AI edit across cells.


Undo AI edits
When focused on a cell container, the Undo command reverts the full set of AI changes at the notebook level.


Text and image output support in chat
You can now add notebook cell outputs, such as text, errors, and images, directly to chat as context. This lets you reference the output when using ask, edit, or agent mode, making it easier for the language model to understand and assist with your notebook content.

Use the Add cell output to chat action, available via the triple-dot menu or by right-clicking the output.

To attach the cell error output as chat context:


To attach the cell output image as chat context:


Accessibility
Chat agent mode improvements
You are now notified when manual action is required during a tool invocation, such as "Run command in terminal." This information is also included in the ARIA label for the relevant chat response, enhancing accessibility for screen reader users.

Additionally, a new accessibility help dialog is available in agent mode, explaining what users can expect from the feature and how to navigate it effectively.

Accessibility Signals for chat edit actions
VS Code now provides auditory signals when you keep or undo AI-generated edits. These signals are configurable via   accessibility.signals.editsKept and   accessibility.signals.editsUndone .

Improved ARIA labels for suggest control
ARIA labels for suggest control items now include richer and descriptive information, such as the type of suggestion (for example, method or variable). This information was previously only available to sighted users via icons.

Source Control
Reference picker improvements
Setting:   git.showReferenceDetails

This milestone, we have made improvements of the reference picker that is used for various source control operations like checkout, merge, rebase, or delete branch. The updated reference picker contains the details of the last commit (author, commit message, commit date), as well as ahead/behind information for local branches. This additional context will help you pick the right reference for the various operations.

Hide the additional information by toggling the   git.showReferenceDetails setting.

Screenshot of the source control references picker showing a list of git branches with details of the last commit, and ahead/behind information.

Repository Status Bar item
Workspaces that contain multiple repositories now have a Source Control Provider Status Bar item that displays the active repository to the left of the branch picker. The new Status Bar item provides additional context, so you know which is the active repository as you navigate between editors and use the Source Control view.

To hide the Source Control Provider Status Bar item, right-click the Status Bar, and deselect Source Control Provider from the context menu.

Screenshot showing the repository status bar item for workspaces that contain more than one repository.

Git blame editor decoration improvements
We have heard feedback that while typing, the "Not Yet Committed" editor decoration does not provide much value and it is more of a distraction. Starting this milestone the "Not Yet Committed" editor decoration is only shown while navigating around the codebase either by using the keyboard or the mouse.

Commit input cursor customization
This milestone, thanks to a community contribution, we have added the   editor.cursorStyle and   editor.cursorWidth settings to the list of settings that are being honored by the source control input box.

Terminal
Reliability in agent mode
The tool that allows agent mode to run commands in the terminal has a number of reliability and compatibility improvements. You should expect fewer cases where the tool gets stuck or where the command finishes without the output being present.

One of the bigger changes is the introduction of the concept of "rich" quality shell integration, as opposed to "basic" and "none". The shell integration scripts shipped with VS Code should generally all enable rich shell integration which provides the best experience in the run in terminal tool (and terminal usage in general). You can view the shell integration quality by hovering over the terminal tab.

Terminal IntelliSense improvements (Preview)
Enhanced IntelliSense for the code CLI
IntelliSense now supports subcommands for the code, code-insiders, and code-tunnel CLI. For instance, typing code tunnel shows available subcommands like help, kill, and prune, each with descriptive info.

Screenshot of the terminal window, showing code tunnel has been typed. The suggest widget shows subcommands like help, kill, prune, and others, with descriptions for each command.

We've also added option suggestions for:

--uninstall-extension
--disable-extension
--install-extension
These show a list of installed extensions to help complete the command.

Screenshot of the VSCode terminal with code --uninstall-extension. A list of available extensions is displayed, including vscode-eslint and editorconfig.

Additionally, code --locate-shell-integration-path now provides shell-specific options such as bash, zsh, fish, and pwsh.

Screenshot of the VSCode terminal showing a command input: code --locate-shell-integration-path with a dropdown menu listing shell options bash, fish, pwsh, and zsh.

Auto-refresh for global commands
The terminal now automatically refreshes its list of global commands when changes are detected in the system bin directory. This means newly installed CLI tools (for example, after running npm install -g pnpm) will show up in completions immediately, without the need to reload the window.

Previously, completions for new tools wouldn’t appear due to caching until the window was manually reloaded.

Option value context
Terminal suggestions now display contextual information about expected option values, helping you more easily complete commands.

Screenshot of the terminal showing a command in progress: npm install --omit. The terminal suggest widget displays <package type> to indicate that's the option that's expected.

Rich completions for fish shell
In the last release, we added detailed command completions for bash and zsh. This iteration, we've expanded that support to fish as well. Completion details are sourced from the shell’s documentation or built-in help commands.

For example, typing jobs in fish displays usage info and options:

Screenshot of the Visual Studio Code Terminal with a fish terminal showing a user has typed jobs. The suggest widget shown provides information about the jobs command with detailed usage examples and options.

File type icons in suggestions
Suggestions in the terminal now include specific icons for different file types, making it easier to distinguish between scripts and binaries at a glance.

Screenshot of the terminal, showing suggestions for various script files, including code.sh, code-cli.sh, and code-server.js. Icons indicate the specific file type.

Inline suggestion details
Inline suggestions, displayed as ghost text in the terminal, continue to appear at the top of the suggestions list. In this release, we've added command details to these entries to provide more context before accepting them.

Screenshot of the terminal, showing the Block command as ghost text in the terminal. The first suggestion is block and it contains usage information.

New simplified and detailed tab hover
By default, the terminal tab shows much less detail now.

Screenshot of the simple hover showing the terminal name, PID, command line, shell integration quality and actions

To view everything, there is a Show Details button at the bottom of the hover.

Screenshot of the detailed hover showing extensions that contribute to the environment and detailed shell integration diagnostics

Signed PowerShell shell integration
The shell integration PowerShell script is now signed, meaning shell integration on Windows when using the default PowerShell execution policy of RemoteSigned should now start working automatically. You can read more about shell integration's benefits here.

Terminal shell type
This iteration, we've finalized our terminal shell API, allowing extensions to see the user's current shell type in their terminal. Subscribing to event onDidChangeTerminalState allows you to see the changes of the user's shell type in the terminal. For example, the shell could change from zsh to bash.

The list of all the shells that are identifiable are currently listed here

Remote Development
Linux legacy server support has ended
As of release 1.99, you can no longer connect to these servers. As noted in our 1.97 release, users that require additional time to complete migration to a supported Linux distro can provide custom builds of glibc and libstdc++ as a workaround. More info on this workaround can be found in the FAQ section.

Enterprise
macOS device management
VS Code now supports device management on macOS in addition to Windows. This allows system administrators to push policies from a centralized management system, like Microsoft Intune.

See the Enterprise Support documentation for more details.

Contributions to extensions
Python
Better support for editable installs with Pylance
Pylance now supports resolving import paths for packages installed in editable mode (pip install -e .) as defined by PEP 660, which enables an improved IntelliSense experience in these scenarios.

This feature is enabled via   python.analysis.enableEditableInstalls and we plan to start rolling it out as the default experience throughout this month. If you experience any issues, please report them at the Pylance GitHub repository.

Faster and more reliable diagnostic experience with Pylance (Experimental)
We're starting to roll out a change to improve the accuracy and responsiveness of Pylance's diagnostics when using the release version of the extension. This is especially helpful for scenarios with multiple open or recently closed files.

If you do not want to wait for the roll-out, you can set   python.analysis.usePullDiagnostics . If you experience any issues, please report them at the Pylance GitHub repository.

Pylance custom Node.js arguments
There's a new   python.analysis.nodeArguments setting, which allows you to pass custom arguments directly to Node.js when using   python.analysis.nodeExecutable . By default, it is set to "--max-old-space-size=8192", but you can modify it to suit your needs (for example, to allocate more memory to Node.js when working with large workspaces).

Additionally, when setting   python.analysis.nodeExecutable to auto, Pylance now automatically downloads Node.js.

Extension authoring
Terminal.shellIntegration tweaks
The Terminal.shellIntegration API will now only light up when command detection happens. Previously, this should work when only the current working directory was reported, which caused TerminalShellIntegration.executeCommand to not function well.

Additionally, TerminalShellIntegration.executeCommand will now behave more consistently and track multiple "sub-executions" for a single command line that ended up running multiple commands. This depends on rich shell integration as mentioned in the reliability in agent mode section.

Proposed APIs
Task problem matcher status
We've added proposed API, so that extensions can monitor when a task's problem matchers start and finish processing lines. Enable it with taskProblemMatcherStatus.

Send images to LLM
This iteration, we've added a proposed API, so that extensions can attach images and send vision requests to the language model. Attachments must be the raw, non base64-encoded binary data of the image (Uint8Array). Maximum image size is 5MB.

Check out this API proposal issue to see a usage example and to stay up to date with the status of this API.

Engineering
Use new /latest API from Marketplace to check for extensions updates
Couple of milestones back, we introduced a new API endpoint in vscode-unpkg service to check for extension updates. Marketplace now supports the same endpoint and VS Code is now using this endpoint to check for extension updates. This is behind an experiment and will be rolled out to users in stages.

Thank you
Last but certainly not least, a big Thank You to the contributors of VS Code.

Issue tracking
Contributions to our issue tracking:

@gjsjohnmurray (John Murray)
@albertosantini (Alberto Santini)
@IllusionMH (Andrii Dieiev)
@RedCMD (RedCMD)
Pull requests
Contributions to vscode:

@a-stewart (Anthony Stewart): Add a 2 hour offset to tests to avoid being a day short after the clocks change PR #243194
@acdzh (Vukk): Fix hasEdits flag's value when updating multiple values in JSONEditingService PR #243876
@c-claeys (Cristopher Claeys): Increment the request attempt in the chat retry action PR #243471
@ChaseKnowlden (Chase Knowlden): Add Merge Editor Accessibility help PR #240745
@dibarbet (David Barbet): Update C# onEnterRules to account for documentation comments PR #242121
@dsanders11 (David Sanders): Fix a few broken @link in vscode.d.ts PR #242407
@jacekkopecky (Jacek Kopecký): Honor more cursor settings in scm input editor PR #242903
@joelverhagen (Joel Verhagen): Show support URL and license URL even if extension URL is not set PR #243565
@kevmo314 (Kevin Wang): Fix comment typo PR #243145
@liudonghua123 (liudonghua): support explorer.copyPathSeparator PR #184884
@mattmaniak: Make telemetry info table a little bit narrower and aligned PR #233961
@notoriousmango (Seong Min Park)
fix: use the copy command for images with CORS errors in the markdown preview PR #240508
Fix Incorrect character indentation on settings with line break PR #242074
@pprchal (Pavel Prchal): Added localization to right-click on icon PR #243679
@SimonSiefke (Simon Siefke): feature: support font family picker in settings PR #214572
@tribals (Anthony): Add discovery of PowerShell Core user installation PR #243025
@tusharsadhwani (Tushar Sadhwani): Make git show ref argument unambiguous PR #242483
@wszgrcy (chen): fix: extension uncaughtException listen Maximum call stack size exceeded PR #244690
@zyoshoka (zyoshoka): Correct typescript-basics extension path PR #243833
Contributions to vscode-css-languageservice:

@lilnasy (Arsh): Support @starting-style PR #421
Contributions to vscode-custom-data:

@rviscomi (Rick Viscomi): Add computed Baseline status PR #111
Contributions to vscode-extension-samples:

@ratmice (matt rice): Fix esbuild scripts PR #1154
Contributions to vscode-extension-telemetry:

@minestarks (Mine Starks): Remove extra brace from common.platformversion PR #221
Contributions to vscode-js-debug:

@erikson84 (Erikson Kaszubowski): fix: ensure backslash in path when setting breakpoints in windows PR #2184
@xymopen (xymopen_Official): Support usenode as npm script runner PR #2178
Contributions to vscode-mypy:

@ivirabyan (Ivan Virabyan): Report only files specified by mypy config PR #352
Contributions to vscode-prompt-tsx:

@dsanders11 (David Sanders): docs: fix sendRequest API name in README example PR #159
@tamuratak (Takashi Tamura)
fix: text should be this.props.text PR #163
Fix the Usage in Tools section in README.md PR #164
Contributions to vscode-pull-request-github:

@aedm (Gábor Gyebnár): Adds sanitizedLowercaseIssueTitle to settings docs PR #6690
Contributions to vscode-python-debugger:

@rchiodo (Rich Chiodo): Update debugpy to latest version PR #653
Contributions to vscode-test:

@SKaplanOfficial (Stephen Kaplan): fix: avoid 'Invalid extraction directory' when unzipping PR #303
Contributions to language-server-protocol:

@MariaSolOs (Maria José Solano): Update range formatting capabilities in metamodel PR #2106
Contributions to python-environment-tools:

@CharlesChen0823: bump winreg PR #195
@elprans (Elvis Pranskevichus)
Avoid misdetecting global Linux Python as virtualenv PR #197
Fix env version detection on systems with multiple system Pythons PR #198
Populate name in virtualenvwrapper envs PR #199
