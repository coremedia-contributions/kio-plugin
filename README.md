# CoreMedia KIO Studio Plugin

The Studio-Plugin consists of a studio-client part. It extends the CoreMedia Studio UI with new AI
functions and connects to the spring-ai-based CoreMedia AI Service running on the same domain as Studio
below the path "/kio".

## Requirements

- CoreMedia Content Cloud **v12 2406.0.3**
-  CoreMedia AI Service, see https://github.com/coremedia-contributions/kio-playground

## Installation

To bundle this plugin with your application Docker images, you can provide plugin-descriptors in the
`plugin-descriptors` directory or reference them as URLs in `plugin-descriptors.json` in the CoreMedia Blueprint workspace.

To extend the CoreMedia Blueprint workspace to bundle a plugin:

1. Go to `workspace-configuration/plugins`.
2. Add the plugin-descriptor (`plugin-descriptors.json`) to the workspace folder.
3. Execute `mvn generate-resources`.
4. Commit the changes.
5. Build the `studio-server`and `studio-client` applications.

You can read more info about Bundling CoreMedia Plugins [here](https://github.com/coremedia-contributions/coremedia-blueprints-workspace/tree/cmcc-12-2406.0.3/workspace-configuration/plugins).

> [!IMPORTANT]
> **Known issue:** It is not possible to bundle the kio-plugin at the moment. Please download and mount the plugin during deployment. See [docs](https://documentation.coremedia.com/cmcc-12/artifacts/2406.0/webhelp/coremedia-en/content/ch04s01s06s03s03s01.html) for more details.
