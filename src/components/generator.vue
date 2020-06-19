<template>
  <v-content>
    <v-row>
      <v-col cols="6">
        <v-card>
          <v-tabs
            v-model="tab"
            background-color="primary"
            centered
            dark
            icons-and-text
          >
            <v-tabs-slider></v-tabs-slider>

            <v-tab v-for="(tab, i) in itemList" :key="i" :href="'#tab-' + i">
              {{ i }}
              <v-icon>mdi-buffer</v-icon>
            </v-tab>
          </v-tabs>

          <v-tabs-items v-model="tab">
            <v-tab-item
              v-for="(tabs, global_key) in itemList"
              :key="global_key"
              :value="'tab-' + global_key"
            >
              <v-card>
                <v-card-text class="left mb-10">
                  <ValidationObserver ref="observer">
                    <form>
                      <h1 class="mb-5">
                        Generate - {{ global_key }} Configuration
                      </h1>
                      <div v-for="(ite, key) in tabs" :key="key">
                        <span
                          v-if="
                            ite.default != ''
                              ? changeModel(ite, global_key)
                              : ''
                          "
                        ></span>
                        <v-row>
                          <v-col cols="10">
                            <ValidationProvider
                              v-if="ite.type == 'String'"
                              v-slot="{ errors }"
                              :name="ite.property"
                              :rules="ite.necesity"
                            >
                              <v-text-field
                                v-model="json[global_key][ite.property]"
                                :error-messages="errors"
                                :label="(ite.necessity==='Required') ? ite.property + '*': ite.property"
                                :data-vv-name="ite.property"
                                outlined
                                @keyup="reloadJsonDOM++"
                              ></v-text-field>
                            </ValidationProvider>

                            <ValidationProvider
                              v-if="ite.type == 'Integer'"
                              v-slot="{ errors }"
                              :name="ite.property"
                              :rules="ite.necesity"
                            >
                              <v-text-field
                                v-model="json[global_key][ite.property]"
                                :error-messages="errors"
                                :label="(ite.necessity==='Required') ? ite.property + '*': ite.property"
                                :data-vv-name="ite.property"
                                outlined
                                @keyup="reloadJsonDOM++"
                              ></v-text-field>
                            </ValidationProvider>
                            <ValidationProvider
                              v-if="ite.type == 'Number'"
                              v-slot="{ errors }"
                              :name="ite.property"
                              :rules="ite.necesity"
                            >
                              <v-text-field
                                v-model="json[global_key][ite.property]"
                                :error-messages="errors"
                                :label="(ite.necessity==='Required') ? ite.property + '*': ite.property"
                                :data-vv-name="ite.property"
                                outlined
                                @keyup="reloadJsonDOM++"
                              ></v-text-field>
                            </ValidationProvider>

                            <ValidationProvider
                              :rules="ite.necesity"
                              :name="ite.property"
                              v-if="ite.type == 'Boolean'"
                            >
                              <v-checkbox
                                v-model="json[global_key][ite.property]"
                                :error-messages="errors"
                                :value="ite.default"
                                :label="(ite.necessity==='Required') ? ite.property + '*': ite.property"
                                :data-vv-name="ite.property"
                                type="checkbox"
                                required
                                @keyup="reloadJsonDOM++"
                              ></v-checkbox>
                            </ValidationProvider>

                            <ValidationProvider
                              v-if="ite.type == 'Array'"
                              v-slot="{ errors }"
                              :name="ite.property"
                              :rules="ite.necesity"
                            >
                              <v-select
                                v-model="json[global_key][ite.property]"
                                :items="ite.values"
                                :error-messages="errors"
                                :label="(ite.necessity==='Required') ? ite.property + '*': ite.property"
                                :data-vv-name="ite.property"
                                outlined
                                @keyup="reloadJsonDOM++"
                              ></v-select>
                            </ValidationProvider>

                            <ValidationProvider
                              :rules="ite.necesity"
                              :name="ite.property"
                              v-if="ite.type == 'Text'"
                            >
                              <v-textarea
                                outlined
                                :name="ite.property"
                                :label="(ite.necessity==='Required') ? ite.property + '*': ite.property"
                                :value="ite.property"
                                v-model="json[global_key][ite.property]"
                                @keyup="reloadJsonDOM++"
                              ></v-textarea>
                            </ValidationProvider>
                          </v-col>
                          <v-col cols="2">
                            <v-btn icon ripple @click="showDesc(ite)">
                              <v-icon color="grey lighten-1"
                                >mdi-information</v-icon
                              >
                            </v-btn>
                          </v-col>
                        </v-row>
                      </div>
                    </form>
                  </ValidationObserver>
                </v-card-text>
              </v-card>
            </v-tab-item>
          </v-tabs-items>
        </v-card>
      </v-col>

      <v-col cols="6">
        <v-card>
          <v-card-text class="left" :key="reloadJsonDOM">
            <vue-json-pretty
              :data="json"
              :path="path"
              :deep="deep"
              :show-double-quotes="showDoubleQuotes"
              :highlight-mouseover-node="highlightMouseoverNode"
              :highlight-selected-node="highlightSelectedNode"
              :show-length="showLength"
              :show-line="showLine"
              :select-on-click-node="selectOnClickNode"
              :collapsed-on-click-brackets="collapsedOnClickBrackets"
              v-model="value"
              :path-selectable="(path, data) => typeof data !== 'number'"
              :selectable-type="selectableType"
              :show-select-controller="showSelectController"
              @click="handleClick(...arguments, 'complexTree')"
              @change="handleChange"
            >
            </vue-json-pretty>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>
          <v-icon large left>
            mdi-information
          </v-icon>
          <small v-text="desc.property" class="title font-weight-light"></small>
        </v-card-title>

        <v-card-text>
          <small v-html="desc.desc" class="body-1"></small>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text color="primary" @click="dialog = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-content>
</template>

<script>
import { required, email, max } from "vee-validate/dist/rules";
import VueJsonPretty from "vue-json-pretty";

import {
  extend,
  ValidationObserver,
  ValidationProvider,
  setInteractionMode
} from "vee-validate";

setInteractionMode("eager");

extend("required", {
  ...required,
  message: "{_field_} can not be empty"
});

extend("max", {
  ...max,
  message: "{_field_} may not be greater than {length} characters"
});

extend("email", {
  ...email,
  message: "Email must be valid"
});

export default {
  components: {
    ValidationProvider,
    ValidationObserver,
    VueJsonPretty
  },
  data: () => ({
    /* Json formater */
    value: "res.error",
    selectableType: "single",
    showSelectController: false,
    showLength: true,
    showLine: true,
    showDoubleQuotes: true,
    highlightMouseoverNode: true,
    highlightSelectedNode: true,
    selectOnClickNode: true,
    collapsedOnClickBrackets: true,
    useCustomLinkFormatter: true,
    path: "res",
    deep: 3,
    itemData: {},
    itemPath: "",
    /* App */
    desc: "",
    itemList: {
       Keybinding: [
        {
          property: "command",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc:
            "The command executed when the associated key bindings are pressed."
        },
        {
          property: "keys",
          type: "Text",
          necessity: "Required",
          default: "",
          values: [],
          desc:
            "Defines the key combinations used to call the command. <a href='https://github.com/microsoft/terminal/blob/master/doc/cascadia/SettingsSchema.md#keys' target='blank'>More info  </a> "
        },
        {
          property: "action",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Adds additional functionality to certain commands."
        }
      ],
      Global: [
        {
          property: "defaultProfile",
          type: "String",
          necessity: "Required",
          default: "PowerShell",
          values: [],
          desc:
            "Sets the default profile. Opens by typing Ctrl + T or by clicking the '+' icon. The guid of the desired default profile is used as the value."
        },
        {
          property: "theme",
          necessity: "Required",
          type: "Array",
          default: "",
          values: ["light", "dark", "system"],
          desc:
            "Sets the theme of the application. Possible values: 'light', 'dark', 'system'"
        },
        {
          property: "disabledProfileSources",
          necessity: "Optional",
          type: "Array",
          default: "",
          values: [
            "Windows.Terminal.Wsl",
            "Windows.Terminal.Azure",
            "Windows.Terminal.PowershellCore"
          ],
          desc:
            "Disables all the dynamic profile generators in this list, preventing them from adding their profiles to the list of profiles on startup. This array can contain any combination of Windows.Terminal.Wsl, Windows.Terminal.Azure, or Windows.Terminal.PowershellCore. For more information, see UsingJsonSettings.md"
        },

        {
          property: "initialCols",
          type: "Integer",
          necessity: "Required",
          default: "120",
          values: [],
          desc: "The number of columns displayed in the window upon first load."
        },
        {
          property: "initialPosition",
          necessity: "Optional",
          type: "String",
          default: ",",
          values: [],
          desc:
            "The position of the top left corner of the window upon first load. On a system with multiple displays, these coordinates are relative to the top left of the primary display. If launchMode is set to 'maximized', the window will be maximized on the monitor specified by those coordinates."
        },
        {
          property: "initialRows",
          necessity: "Required",
          type: "Integer",
          default: "30",
          values: [],
          desc: "The number of rows displayed in the window upon first load."
        },
        {
          property: "launchMode",
          necessity: "Optional",
          type: "String",
          default: "default",
          values: [],
          desc:
            "Defines whether the Terminal will launch as maximized or not. Possible values: 'default', 'maximized'"
        },
        {
          property: "rowsToScroll",
          necessity: "Optional",
          type: "Integer",
          default: "system",
          values: [],
          desc:
            "The number of rows to scroll at a time with the mouse wheel. This will override the system setting if the value is not zero or 'system'."
        },

        {
          property: "tabWidthMode",
          necessity: "Optional",
          type: "String",
          default: "equal",
          values: [],
          desc:
            "Sets the width of the tabs. Possible values: <br><b>'equal'</b>: sizes each tab to the same width <br><b>'titleLength'</b>: sizes each tab to the length of its title <br><b>'compact'</b>: sizes each tab to the length of its title when focused, and shrinks to the size of only the icon when the tab is unfocused."
        },
        {
          property: "wordDelimiters	",
          necessity: "Optional",
          type: "String",
          default: "",
          values: [],
          desc: "Determines the delimiters used in a double click selection."
        },
        {
          property: "confirmCloseAllTabs	",
          necessity: "Optional",
          type: "Boolean",
          default: true,
          values: [],
          desc:
            "When set to true closing a window with multiple tabs open WILL require confirmation. When set to false closing a window with multiple tabs open WILL NOT require confirmation."
        },
        {
          property: "startOnUserLogin",
          necessity: "Optional",
          type: "Boolean",
          default: false,
          values: [],
          desc:
            "When set to true enables the launch of Windows Terminal at startup. Setting to false will disable the startup task entry. Note: if the Windows Terminal startup task entry is disabled either by org policy or by user action this setting will have no effect."
        },
        {
          property: "experimental.rendering.forceFullRepaint",
          necessity: "Optional",
          type: "Boolean",
          default: false,
          values: [],
          desc:
            "When set to true, we will redraw the entire screen each frame. When set to false, we will render only the updates to the screen between frames."
        },
        {
          property: "experimental.rendering.software",
          necessity: "Optional",
          type: "Boolean",
          default: false,
          values: [],
          desc:
            "When set to true, we will use the software renderer (a.k.a. WARP) instead of the hardware one."
        },
        {
          property: "alwaysShowTabs",
          type: "Boolean",
          necessity: "Required",
          default: true,
          values: [],
          desc:
            "When set to true, tabs are always displayed. When set to false and showTabsInTitlebar is set to false, tabs only appear after typing Ctrl + T."
        },
        {
          property: "copyOnSelect",
          type: "Boolean",
          necessity: "Optional",
          default: false,
          values: [],
          desc:
            "When set to true, a selection is immediately copied to your clipboard upon creation. When set to false, the selection persists and awaits further action."
        },
        {
          property: "copyFormatting",
          type: "Boolean",
          necessity: "Optional",
          default: false,
          values: [],
          desc:
            "When set to true, the color and font formatting of selected text is also copied to your clipboard. When set to false, only plain text is copied to your clipboard."
        },
        {
          property: "showTerminalTitleInTitlebar",
          necessity: "Required",
          type: "Boolean",
          default: true,
          values: [],
          desc:
            "When set to true, titlebar displays the title of the selected tab. When set to false, titlebar displays 'Windows Terminal'."
        },
        {
          property: "showTabsInTitlebar",
          necessity: "Optional",
          type: "Boolean",
          default: true,
          values: [],
          desc:
            "When set to true, the tabs are moved into the titlebar and the titlebar disappears. When set to false, the titlebar sits above the tabs."
        },
        {
          property: "snapToGridOnResize",
          necessity: "Optional",
          type: "Boolean",
          default: false,
          values: [],
          desc:
            "When set to true, the window will snap to the nearest character boundary on resize. When false, the window will resize 'smoothly'"
        }
      ],
      Profile: [
        {
          property: "guid",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc:
            "Unique identifier of the profile. Written in registry format: '{00000000-0000-0000-0000-000000000000}'."
        },
        {
          property: "name",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc:
            "Name of the profile. Displays in the dropdown menu. Additionally, this value will be used as the 'title' to pass to the shell on startup. Some shells (like bash) may choose to ignore this initial value, while others (cmd, powershell) may use this value over the lifetime of the application. This 'title' behavior can be overridden by using tabTitle."
        },
        {
          property: "acrylicOpacity",
          type: "Number",
          necessity: "Optional",
          default: "0.5",
          values: [],
          desc:
            "When useAcrylic is set to true, it sets the transparency of the window for the profile. Accepts floating point values from 0-1."
        },
        {
          property: "antialiasingMode",
          type: "Array",
          necessity: "Optional",
          default: "grayscale",
          values: ["grayscale", "cleartype", "aliased"],
          desc:
            "Controls how text is antialiased in the renderer. Possible values are 'grayscale', 'cleartype' and 'aliased'. Note that changing this setting will require starting a new terminal instance."
        },
        {
          property: "background",
          type: "String",
          necessity: "Optional",
          default: "rrggbb",
          values: [],
          desc:
            "Sets the background color of the profile. Overrides background set in color scheme if colorscheme is set. Uses hex color format: '#rrggbb'."
        },
        {
          property: "backgroundImage",
          type: "String",
          necessity: "Optional",
          default: "",
          values: [],
          desc:
            "Sets the file location of the Image to draw over the window background."
        },
        {
          property: "backgroundImageAlignment",
          type: "Array",
          necessity: "Optional",
          default: "center",
          values: [
            "center",
            "left",
            "top",
            "right",
            "bottom",
            "topLeft",
            "topRight",
            "bottomLeft",
            "bottomRight"
          ],
          desc:
            "Sets how the background image aligns to the boundaries of the window."
        },
        {
          property: "backgroundImageOpacity",
          type: "Number",
          necessity: "Optional",
          default: "1.0",
          values: [],
          desc:
            "Sets the transparency of the background image. Accepts floating point values from 0-1."
        },
        {
          property: "backgroundImageStretchMode",
          type: "Array",
          necessity: "Optional",
          default: "uniformToFill",
          values: ["none", "fill", "uniform", "uniformToFill"],
          desc: "Sets how the background image is resized to fill the window. "
        },
        {
          property: "closeOnExit",
          type: "String",
          necessity: "Optional",
          default: "graceful",
          values: ["graceful", "always", "never"],
          desc:
            "Sets how the profile reacts to termination or failure to launch. Possible values: 'graceful' (close when exit is typed or the process exits normally), 'always' (always close) and 'never' (never close). true and false are accepted as synonyms for 'graceful' and 'never' respectively."
        },
        {
          property: "colorScheme",
          type: "String",
          necessity: "Optional",
          default: "Campbell",
          values: [],
          desc:
            "Name of the terminal color scheme to use. Color schemes are defined under schemes."
        },
        {
          property: "commandline",
          type: "String",
          necessity: "Optional",
          default: "",
          values: [],
          desc: "Executable used in the profile."
        },
        {
          property: "cursorColor",
          type: "String",
          necessity: "Optional",
          default: "",
          values: [],
          desc:
            "Sets the cursor color of the profile. Overrides cursorColor set in color scheme if colorscheme is set. Uses hex color format: '#rrggbb'."
        },
        {
          property: "cursorHeight",
          type: "Integer",
          necessity: "Optional",
          default: "",
          values: [],
          desc:
            "Sets the percentage height of the cursor starting from the bottom. Only works when cursorShape is set to 'vintage'. Accepts values from 25-100."
        },
        {
          property: "cursorShape",
          type: "Array",
          necessity: "Optional",
          default: "bar",
          values: ["vintage", "bar", "underscore", "filledBox", "emptyBox"],
          desc: "Sets the cursor shape for the profile."
        },
        {
          property: "fontFace",
          type: "String",
          necessity: "Optional",
          default: "Cascadia Mono	",
          values: [],
          desc:
            "Name of the font face used in the profile. We will try to fallback to Consolas if this can't be found or is invalid."
        },
        {
          property: "fontWeight",
          type: "Array",
          necessity: "Optional",
          default: "normal",
          values: [
            "thin",
            "extra-light",
            "light",
            "semi-light",
            "normal",
            "medium",
            "semi-bold",
            "bold",
            "extra-bold",
            "black",
            "extra-black"
          ],
          desc:
            "Sets the weight (lightness or heaviness of the strokes) for the given font"
        },
        {
          property: "foreground",
          type: "String",
          necessity: "Optional",
          default: "",
          values: [],
          desc:
            "Sets the foreground color of the profile. Overrides foreground set in color scheme if colorscheme is set. Uses hex color format: #rgb or '#rrggbb'."
        },
        {
          property: "hidden",
          type: "Boolean",
          necessity: "Optional",
          default: false,
          values: [],
          desc:
            "If set to true, the profile will not appear in the list of profiles. This can be used to hide default profiles and dynamically generated profiles, while leaving them in your settings file."
        },
        {
          property: "historySize",
          type: "Integer",
          necessity: "Optional",
          default: "9001",
          values: [],
          desc:
            "The number of lines above the ones displayed in the window you can scroll back to."
        },
        {
          property: "icon",
          type: "String",
          necessity: "Optional",
          default: "",
          values: [],
          desc:
            "Image file location of the icon used in the profile. Displays within the tab and the dropdown menu."
        },
        {
          property: "padding",
          type: "String",
          necessity: "Optional",
          default: "8, 8, 8, 8",
          values: [],
          desc:
            "Sets the padding around the text within the window. Can have three different formats: '#' sets the same padding for all sides, '#, #' sets the same padding for left-right and top-bottom, and '#, #, #, #' sets the padding individually for left, top, right, and bottom."
        },
        {
          property: "scrollbarState",
          type: "Array",
          necessity: "Optional",
          default: "visible",
          values: ["visible", "hidden"],
          desc: "Defines the visibility of the scrollbar. "
        },
        {
          property: "selectionBackground",
          type: "String",
          necessity: "Optional",
          default: "",
          values: [],
          desc:
            "Sets the selection background color of the profile. Overrides selectionBackground set in color scheme if colorscheme is set. Uses hex color format: '#rrggbb'."
        },
        {
          property: "snapOnInput",
          type: "Boolean",
          necessity: "Optional",
          default: true,
          values: [],
          desc:
            "When set to true, the window will scroll to the command input line when typing. When set to false, the window will not scroll when you start typing."
        },
        {
          property: "altGrAliasing",
          type: "Boolean",
          necessity: "Optional",
          default: true,
          values: [],
          desc:
            "By default Windows treats Ctrl+Alt as an alias for AltGr. When altGrAliasing is set to false, this behavior will be disabled."
        },
        {
          property: "source",
          type: "String",
          necessity: "Optional",
          default: "",
          values: [],
          desc:
            "Stores the name of the profile generator that originated this profile. There are no discoverable values for this field."
        },
        {
          property: "startingDirectory",
          type: "String",
          necessity: "Optional",
          default: "%USERPROFILE%",
          values: [],
          desc: "The directory the shell starts in when it is loaded."
        },
        {
          property: "suppressApplicationTitle",
          type: "Boolean",
          necessity: "Optional",
          default: false,
          values: [],
          desc:
            "When set to true, tabTitle overrides the default title of the tab and any title change messages from the application will be suppressed. When set to false, tabTitle behaves as normal."
        },
        {
          property: "tabTitle",
          type: "String",
          necessity: "Optional",
          default: "",
          values: [],
          desc:
            "If set, will replace the name as the title to pass to the shell on startup. Some shells (like bash) may choose to ignore this initial value, while others (cmd, powershell) may use this value over the lifetime of the application."
        },
        {
          property: "useAcrylic",
          type: "Boolean",
          necessity: "Optional",
          default: false,
          values: [],
          desc:
            "When set to true, the window will have an acrylic background. When set to false, the window will have a plain, untextured background. The transparency only applies to focused windows due to OS limitation."
        },
        {
          property: "experimental.retroTerminalEffect	",
          type: "Boolean",
          necessity: "Optional",
          default: false,
          values: [],
          desc:
            "When set to true, enable retro terminal effects. This is an experimental feature, and its continued existence is not guaranteed."
        }
      ],
      Schema: [
        {
          property: "name",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Name of the color scheme."
        },
        {
          property: "foreground",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the foreground color of the color scheme."
        },
        {
          property: "background",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the background color of the color scheme."
        },
        {
          property: "selectionBackground",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the selection background color of the color scheme."
        },
        {
          property: "cursorColor",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the cursor color of the color scheme."
        },
        {
          property: "black",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI black."
        },
        {
          property: "blue",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI blue."
        },
        {
          property: "brightBlack",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI bright black."
        },
        {
          property: "brightBlue",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI bright blue."
        },
        {
          property: "brightCyan",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI bright cyan."
        },
        {
          property: "brightGreen",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI bright green."
        },
        {
          property: "brightPurple",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI bright purple."
        },
        {
          property: "brightRed",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI bright red."
        },
        {
          property: "brightWhite",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI bright white."
        },
        {
          property: "brightYellow",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI bright yellow."
        },
        {
          property: "cyan",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI cyan."
        },
        {
          property: "green",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI green."
        },
        {
          property: "purple",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI purple."
        },
        {
          property: "red",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI red."
        },
        {
          property: "white",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI white."
        },
        {
          property: "yellow",
          type: "String",
          necessity: "Required",
          default: "",
          values: [],
          desc: "Sets the color used as ANSI yellow."
        }
      ]
     
    },
    tab: null,
    dialog: false,
    json: {
      Global: {},
      Profile: {},
      Keybinding: {},
      Schema: {}
    },
    errors: "",
    reloadJsonDOM: 1
  }),

  methods: {
    handleClick(path, data, treeName = "") {
      console.log("click: ", path, data, treeName);
      this.itemPath = path;
      this.itemData = !data ? data + "" : data;
    },
    handleChange(newVal, oldVal) {
      console.log("newVal: ", newVal, " oldVal: ", oldVal);
    },
    customLinkFormatter(data) {
      if (data.startsWith("http://")) {
        return `<a style="color:red;" href="${data}">"${data}"</a>`;
      } else {
        return null;
      }
    },
    changeModel(ite, global_key) {
      if (this.json[global_key][ite.property] == undefined) {
        this.json[global_key][ite.property] = ite.default;
      }
    },
    showDesc(description) {
      this.desc = description;
      this.dialog = true;
    }
  }
};
</script>

<style>
.left {
  text-align: left;
}
.first_row {
  background-color: #cccccc3b;
}
</style>
