# Release process
- update `version` in `package.json` and describe changes in `CHANGELOG.md`
- install `vsce`: install [Node.js](https://nodejs.org/en) and run `npm install -g @vscode/vsce` ([Visual Studio Code guide](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#vsce))
- run `vsce package` inside the repo's root directory 
- go to the [publishers list](https://marketplace.visualstudio.com/manage/publishers/typedb), press `...` near `TypeQL` and upload the generated `.vsix` file via `Update`.
