<template>
  <div>
    <v-content class="first_row">
      <v-container>
        <v-row class="row_">
          <v-col cols="6">
            <h1 class="h1 mt-5">Windows Terminal Settings Generator</h1>
            <h2 class="h2 mt-3">
              Settings Generator for Windows Terminal the modern, fast,
              efficient, powerful, and productive terminal application.
            </h2>
            <v-btn class="mt-10 primary" href="/generator">
              Gnerate settings
            </v-btn>
          </v-col>

          <v-col cols="6">
            <img
              class="terminal_img"
              src="https://www.softzone.es/app/uploads-softzone.es/2020/05/Windows-Terminal-con-efecto-acr%C3%ADlico-800x419.jpg"
              width="100%"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-content>
    <v-content class="">
      <v-container>
        <v-row class="row_">
          <v-col cols="12 center">
            <h1 class="h1 ma-10">What is Windows Terminal?</h1>
          </v-col>
          <v-col cols="6">
            <img
              src="https://2vplsu2wo1292qcj331fo1gj-wpengine.netdna-ssl.com/wp-content/uploads/2019/06/windows_terminal_icon-1-728x409.jpg"
              width="100%"
            />
          </v-col>
          <v-col cols="6">
            <v-card>
              <v-card-text>
                <div class="card_text">
                  <p>
                    The Windows Terminal is a modern, fast, efficient, powerful,
                    and productive terminal application for users of
                    command-line tools and shells like Command Prompt,
                    PowerShell, and WSL.
                  </p>
                  <p>
                    Its main features include multiple tabs, panes, Unicode and
                    UTF-8 character support, a GPU accelerated text rendering
                    engine, and custom themes, styles, and configurations.
                  </p>
                  <p>
                    This is an open source project and we welcome community
                    participation. To participate please visit
                    https://github.com/microsoft/terminal
                  </p>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </div>
</template>

<script>
export default { name: "Home" };
</script>

<style>
.h1 {
  font-size: 3rem;
  font-weight: 900;
  line-height: 3.5rem;
}
.h2,
.card_text {
  font-size: 1.2rem;
  font-weight: 400;
}
.card_text {
  line-height: 2rem;
}
.terminal_img {
  border-radius: 8px;
}
.first_row {
  background-color: #cccccc3b;
}
.center {
  text-align: center;
}
.row_ {
  padding-bottom: 50px;
  padding-top: 50px;
}
</style>
