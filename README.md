# Elixir Google Spreadsheets
Elixir library to read and write data of Google Spreadsheets.

This library is based on __Google Cloud API v4__ and uses __Google Service Accounts__ to manage it's content.

# Setup
1. Use [this](https://console.developers.google.com/start/api?id=sheets.googleapis.com) wizard to create or select a project in the Google Developers Console and automatically turn on the API. Click __Continue__, then __Go to credentials__.
2. On the __Add credentials to your project page__, create __Service account key__.
3. Select your project name as service account and __JSON__ as key format, download the created key and rename it to __service_account.json__.
4. Press __Manage service accounts__ on a credential page, copy your __Service Account Identifier__: _[projectname]@[domain].iam.gserviceaccount.com_
5. Create or open existing __Google Spreadsheet document__ on your __Google Drive__ and add __Service Account Identifier__ as user invited in spreadsheet's __Collaboration Settings__.
6. Add `{:elixir_google_spreadsheets, git: "https://github.com/Voronchuk/elixir_google_spreadsheets.git"}` to __mix.exs__ under `deps` function, add `:elixir_google_spreadsheets` in your application list.
7. Add __service_account.json__ in your `config.exs` or other config file, like `dev.exs` or `prod.secret.exs`.
    config :goth,
        json: "./config/service_account.json" |> File.read!
8. Run `mix deps.get && mix deps.compile`.

# Usage
Initialise spreadsheet thread with it's id which you can fetch from URL:

    `{:ok, pid} = GSS.Spreadsheet.Supervisor.spreadsheet("XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX-XXXXXXXXX")`

Sample operations:

* `GSS.Spreadsheet.id(pid)`
* `GSS.Spreadsheet.rows(pid)`
* `GSS.Spreadsheet.read_row(pid, 1, column_to: 5)`
* `GSS.Spreadsheet.write_row(pid, 1, ["1", "2", "3", "4", "5"])`
* `GSS.Spreadsheet.append_row(pid, 1, ["1", "2", "3", "4", "5"])`
* `GSS.Spreadsheet.clear_row(pid, 1)`

Last function param of `GSS.Spreadsheet` function calls support the same `Keyword` options (in snake_case instead of camelCase), as defined in [Google API Docs](https://developers.google.com/sheets/reference/rest/v4/spreadsheets.values).

We also define `column_from` and `column_to` Keyword options which control range of cell which will be queried.

Default values:
* `column_from = 1`
* `column_to = 25`
* `major_dimension = "ROWS"`
* `value_render_option = "FORMATTED_VALUE"`
* `datetime_render_option = "FORMATTED_STRING"`
* `value_render_option = "USER_ENTERED"`
* `insert_data_option` = "INSERT_ROWS"

# Restrictions
* We don't support multi row operations at the moment;
* Batch queries `batchGet`, `batchClear`, `batchUpdate` are not implemented;
* __This library is in it's early beta, use on your own risk. Pull requests / reports / feedback are welcome.__
