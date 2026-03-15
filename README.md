This is for educational purposes not harrasment its just a silly little betterdiscord plugin. Drag and drop it into your betterdiscord plugins folder and enable it. 
Sending the msg "kys" in any dm or server channel triggers a fun little messege. Test this out in a private server/dm first.









/**
 * @name KYSLink
 * @version 1.0.0
 * @description Replaces "kys" with a special little messege for whomever is bothering you, works in dms and server channels.
 * @author Dusaid
 */

module.exports = (_ => {
    const config = {
        info: {
            name: "KYSLink",
            author: "You",
            version: "1.0.0",
            description: "Replaces 'kys' with a Wikipedia link before sending"
        }
    };

    return !window.BDFDB_Global || (!window.BDFDB_Global.loaded && !window.BDFDB_Global.started) ? class {
        getName() {return config.info.name;}
        getAuthor() {return config.info.author;}
        getVersion() {return config.info.version;}
        getDescription() {return `The BDFDB Library plugin is required for ${config.info.name}.`;}
        load() {BdApi.showToast("KYSLink requires BDFDB Library!", {type: "error"});}
        start() {this.load();}
        stop() {}
        getSettingsPanel() {return document.createElement("div");}
    } : (([Plugin, BDFDB]) => {

        return class KYSLink extends Plugin {

            onStart() {
                this.patchSendMessage();
                console.log("KYSLink started");
            }

            onStop() {
                BdApi.Patcher.unpatchAll(this.getName());
                console.log("KYSLink stopped");
            }

            patchSendMessage() {
                BDFDB.PatchUtils.patch(this, BDFDB.LibraryModules.MessageUtils, "sendMessage", {instead: e => {
                    const content = e.methodArguments[1].content;
                    if (content.trim().toLowerCase() === "kys") {
                        e.methodArguments[1].content = "Hope this helps! :3 https://en.wikipedia.org/wiki/Suicide_methods";
                    }
                    e.callOriginalMethodAfterwards();
                }});
            }
        };
    })(window.BDFDB_Global.PluginUtils.buildPlugin(config));
})();
