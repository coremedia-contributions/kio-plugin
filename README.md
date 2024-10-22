# CoreMedia KIO Studio Plugin

The Studio-Plugin consists of 2 parts, a studio-server part and a studio-client part.

The server part is needed to address CORS for accessing an external system
in CoreMedia Studio. It basically allows the CoreMedia Studio Client app to access the spring-ai-based
CoreMedia AI Service.

The client part extends the CoreMedia Studio UI with new AI functions.

## Installation

To bundle this plugin with your application Docker images, you can provide plugin-descriptors in the
`plugin-descriptors` directory directly in each app (`studio-server` and `studio-client`)
or reference them as URLs in `plugin-descriptors.json` in the CoreMedia Blueprint workspace.

To extend the CoreMedia Blueprint workspace to bundle a plugin:

1. Go to `workspace-configuration/plugins`.
2. Add the plugin-descriptor (`plugin-descriptors.json`) to the workspace folder.
3. Execute `mvn generate-resources`.
4. Commit the changes.
5. Build the `studio-server`and `studio-client` applications.

You can read more info about Bundling CoreMedia Plugins [here](https://github.com/coremedia-contributions/coremedia-blueprints-workspace/tree/cmcc-12-2406.0.2/workspace-configuration/plugins).

## Configuration

The _CoreMedia KIO Studio Plugin_ can be configured via CoreMedia content settings. Currently, it is mainly used to
configure 3 things: The AI Service Connection, the supported content fields and the AI prompts. 

### AI Service Connection

The default AI service backend connection can be overwritten (default is `https://kio.cloud.coremedia.io/api/v1/chat`
and is for all customers who are using the central AI services provided by CoreMedia). Customers with their own
customized version of the AI Service would overwrite this connection. There are 3 options to set:
- `streamingUrl`: the streaming endpoint of the CoreMedia AI Service (default is `https://kio.cloud.coremedia.io/api/v1/chat`)
- `requestUrl`: another endpoint all for all non-interactive requests (default is `https://kio.cloud.coremedia.io/api/v1/chat/request`),
if the urls is simply the `streamingUrl` plus a `/request` suffix this setting can be omitted
- `authHeader`: an authentication header that should be used for communication with the CoreMedia AI Service

### Supported Fields

The supported content fields for AI data generation can be overwritten.
There is a restriction that only string fields of content type `CMLinkable` and its subtypes are supported.

Currently, this feature supports the generation of HTML metadata. This means the feature is active by default for
these fields: `htmlTitle`, `htmlDescription` and `keywords`. This list can be modified using the content settings.

**Please note that for new fields, only fields of type string are supported that are contained in `CMLinkable`
or subtypes. And please also note that some resource keys might be missing (for localized display of labels)
if they are missing in the resource bundle provided by CoreMedia.**

### AI Prompts

The default AI prompts that will be sent to the AI Service (or at least some of its parts) for each content field
can be set. There are parts of the system prompt that cannot be overwritten via settings. These are predefined by CoreMedia.
However, a more precise return format description can be defined in a setting named `formatDescription`.

If you experience issues with the AI responses, for example, if the answers often come in a format that cannot be
directly applied to the content you can change the `formatDescription` of the field.

Also, the exact `userPrompt` that will be sent to the AI can be defined by a setting for each field.
The value should be short and concise because all the format restrictions have already been defined in `formatDescription`.

### Settings lookup path

There are 2 places where a settings document is searched for: in CoreMedia site settings folder and in the global
settings folder. The settings documents must be named exactly `KIO`, and stored directly in the folder `Settings`
(the format can be seen in the example in the next section).

The settings are therefore searched for in exactly these two places:
- Site-specific: `<Site-folder>/Options/Settings/KIO`
- Global: `/Settings/Options/Settings/KIO`

**Note, it is not necessary to link the document from the root channel (_Linked Settings_). It has no effect.**

**Please also note, typically it is not necessary to have different site settings just to have responses from AI in
different languages. Setting the response language is already part of the hard-coded system prompt. It would only be
necessary to "localize" the `userPrompt` in Site-specific settings if the editor was bothered by the English user prompt
in the UI. But this typically does not affect the functionality.**

### Example settings

Please note, the settings shown here are already included in the system (hard-coded).
It is only necessary to create them if you want to change them at some point.

```
<?xml version="1.0" encoding="UTF-8" ?>
<CMSettings folder="/Settings/Options/Settings" name="KIO" xmlns:cmexport="http://www.coremedia.com/2012/cmexport">
  <externalRefId></externalRefId>
  <locale></locale>
  <master/>
  <settings><Struct xmlns="http://www.coremedia.com/2008/struct" xmlns:xlink="http://www.w3.org/1999/xlink">
    <StructProperty Name="kio">
      <Struct>
        <StructProperty Name="chatApi">
          <Struct>
<StringProperty Name="streamingUrl">https://kio.cloud.coremedia.io/api/v1/chat</StringProperty>
<StringProperty Name="requestUrl">https://kio.cloud.coremedia.io/api/v1/chat/request</StringProperty>
<StringProperty Name="authHeader">&lt;change-it&gt;</StringProperty>
          </Struct>
        </StructProperty>
        <StructListProperty Name="fields">
          <Struct>
<BooleanProperty Name="enabled">true</BooleanProperty>
<StringProperty Name="contentType">CMLinkable</StringProperty>
<StringProperty Name="property">htmlTitle</StringProperty>
<StringProperty Name="formatDescription">Create plain text that can be used as "HTML title" in a web page with a maximum length of 100 characters.</StringProperty>
<StringProperty Name="userPrompt">Create a HTML Title.</StringProperty>
          </Struct>
          <Struct>
<BooleanProperty Name="enabled">true</BooleanProperty>
<StringProperty Name="contentType">CMLinkable</StringProperty>
<StringProperty Name="property">htmlDescription</StringProperty>
<StringProperty Name="formatDescription">Create plain text that can be used as "HTML meta description" in a web page with a maximum length of 400 characters.</StringProperty>
<StringProperty Name="userPrompt">Create a HTML Meta Description.</StringProperty>
          </Struct>
          <Struct>
<BooleanProperty Name="enabled">true</BooleanProperty>
<StringProperty Name="contentType">CMLinkable</StringProperty>
<StringProperty Name="property">keywords</StringProperty>
<StringProperty Name="formatDescription">Create a comma-separated list of lower-case keywords with maximum length of 900 characters.</StringProperty>
<StringProperty Name="userPrompt">Create 5 Free Keywords.</StringProperty>
          </Struct>
        </StructListProperty>
      </Struct>
    </StructProperty>
  </Struct></settings>
  <identifier></identifier>
</CMSettings>
```
