---
description: Learn how to build a nameservice application (continued)
---

# Nameservice Module CLI

## Nameservice Module CLI <a id="nameservice-module-cli"></a>

The Cosmos SDK uses the [`cobra`](https://github.com/spf13/cobra) library for CLI interactions. This library makes it easy for each module to expose its own commands. To get started defining the user's CLI interactions with your module, create the following files:

* `./x/nameservice/client/cli/query.go`
* `./x/nameservice/client/cli/tx.go`

### Queries <a id="queries"></a>

Start in `query.go`. Here, define `cobra.Command`s for each of your modules `Queriers` \(`resolve`, and `whois`\):

```javascript
package cli

import (
    "fmt"

    "github.com/cosmos/cosmos-sdk/client"
    "github.com/cosmos/cosmos-sdk/client/context"
    "github.com/cosmos/cosmos-sdk/client/flags"
    "github.com/cosmos/cosmos-sdk/codec"
    "github.com/cosmos/sdk-tutorials/nameservice/x/nameservice/types"
    "github.com/spf13/cobra"
)

func GetQueryCmd(storeKey string, cdc *codec.Codec) *cobra.Command {
    nameserviceQueryCmd := &cobra.Command{
        Use:                        types.ModuleName,
        Short:                      "Querying commands for the nameservice module",
        DisableFlagParsing:         true,
        SuggestionsMinimumDistance: 2,
        RunE:                       client.ValidateCmd,
    }
    nameserviceQueryCmd.AddCommand(flags.GetCommands(
        GetCmdResolveName(storeKey, cdc),
        GetCmdWhois(storeKey, cdc),
        GetCmdNames(storeKey, cdc),
    )...)

    return nameserviceQueryCmd
}

// GetCmdResolveName queries information about a name
func GetCmdResolveName(queryRoute string, cdc *codec.Codec) *cobra.Command {
    return &cobra.Command{
        Use:   "resolve [name]",
        Short: "resolve name",
        Args:  cobra.ExactArgs(1),
        RunE: func(cmd *cobra.Command, args []string) error {
            cliCtx := context.NewCLIContext().WithCodec(cdc)
            name := args[0]

            res, _, err := cliCtx.QueryWithData(fmt.Sprintf("custom/%s/resolve/%s", queryRoute, name), nil)
            if err != nil {
                fmt.Printf("could not resolve name - %s \n", name)
                return nil
            }

            var out types.QueryResResolve
            cdc.MustUnmarshalJSON(res, &out)
            return cliCtx.PrintOutput(out)
        },
    }
}

// GetCmdWhois queries information about a domain
func GetCmdWhois(queryRoute string, cdc *codec.Codec) *cobra.Command {
    return &cobra.Command{
        Use:   "whois [name]",
        Short: "Query whois info of name",
        Args:  cobra.ExactArgs(1),
        RunE: func(cmd *cobra.Command, args []string) error {
            cliCtx := context.NewCLIContext().WithCodec(cdc)
            name := args[0]

            res, _, err := cliCtx.QueryWithData(fmt.Sprintf("custom/%s/whois/%s", queryRoute, name), nil)
            if err != nil {
                fmt.Printf("could not resolve whois - %s \n", name)
                return nil
            }

            var out types.Whois
            cdc.MustUnmarshalJSON(res, &out)
            return cliCtx.PrintOutput(out)
        },
    }
}

// GetCmdNames queries a list of all names
func GetCmdNames(queryRoute string, cdc *codec.Codec) *cobra.Command {
    return &cobra.Command{
        Use:   "names",
        Short: "names",
        // Args:  cobra.ExactArgs(1),
        RunE: func(cmd *cobra.Command, args []string) error {
            cliCtx := context.NewCLIContext().WithCodec(cdc)

            res, _, err := cliCtx.QueryWithData(fmt.Sprintf("custom/%s/names", queryRoute), nil)
            if err != nil {
                fmt.Printf("could not get query names\n")
                return nil
            }

            var out types.QueryResNames
            cdc.MustUnmarshalJSON(res, &out)
            return cliCtx.PrintOutput(out)
        },
    }
}
```

Notes on the above code:

* The CLI introduces a new `context`: [`CLIContext`](https://godoc.org/github.com/cosmos/cosmos-sdk/client/context#CLIContext). It carries data about user input and application configuration that are needed for CLI interactions.
* The `path` required for the `cliCtx.QueryWithData()` function maps directly to the names in your query router.
  * The first part of the path is used to differentiate the types of queries possible to SDK applications: `custom` is for `Queriers`.
  * The second piece \(`nameservice`\) is the name of the module to route the query to.
  * Finally there is the specific querier in the module that will be called.
  * In this example the fourth piece is the query. This works because the query parameter is a simple string. To enable more complex query inputs you need to use the second argument of the [`.QueryWithData()`](https://godoc.org/github.com/cosmos/cosmos-sdk/client/context#CLIContext.QueryWithData) function to pass in `data`. For an example of this see the [queriers in the Staking module](https://github.com/cosmos/cosmos-sdk/blob/5af6bd77aa6c0e8facc936947a3365416892e44d/x/staking/keeper/querier.go).

### Transactions <a id="transactions"></a>

Now that the query interactions are defined, it is time to move on to transaction generation in `tx.go`:

> _NOTE_: Your application needs to import the code you just wrote. Here the import path is set to this repository \(`github.com/cosmos/sdk-tutorials/nameservice/x/nameservice`\). If you are following along in your own repo you will need to change the import path to reflect that \(`github.com/{ .Username }/{ .Project.Repo }/x/nameservice`\).

```javascript
package cli

import (
    "bufio"

    "github.com/spf13/cobra"

    "github.com/cosmos/cosmos-sdk/client"
    "github.com/cosmos/cosmos-sdk/client/context"
    "github.com/cosmos/cosmos-sdk/client/flags"
    "github.com/cosmos/cosmos-sdk/codec"
    sdk "github.com/cosmos/cosmos-sdk/types"
    "github.com/cosmos/cosmos-sdk/x/auth"
    "github.com/cosmos/cosmos-sdk/x/auth/client/utils"
    "github.com/cosmos/sdk-tutorials/nameservice/x/nameservice/types"
)

func GetTxCmd(storeKey string, cdc *codec.Codec) *cobra.Command {
    nameserviceTxCmd := &cobra.Command{
        Use:                        types.ModuleName,
        Short:                      "Nameservice transaction subcommands",
        DisableFlagParsing:         true,
        SuggestionsMinimumDistance: 2,
        RunE:                       client.ValidateCmd,
    }

    nameserviceTxCmd.AddCommand(flags.PostCommands(
        GetCmdBuyName(cdc),
        GetCmdSetName(cdc),
        GetCmdDeleteName(cdc),
    )...)

    return nameserviceTxCmd
}

// GetCmdBuyName is the CLI command for sending a BuyName transaction
func GetCmdBuyName(cdc *codec.Codec) *cobra.Command {
    return &cobra.Command{
        Use:   "buy-name [name] [amount]",
        Short: "bid for existing name or claim new name",
        Args:  cobra.ExactArgs(2),
        RunE: func(cmd *cobra.Command, args []string) error {
            inBuf := bufio.NewReader(cmd.InOrStdin())
            cliCtx := context.NewCLIContext().WithCodec(cdc)

            txBldr := auth.NewTxBuilderFromCLI(inBuf).WithTxEncoder(utils.GetTxEncoder(cdc))

            coins, err := sdk.ParseCoins(args[1])
            if err != nil {
                return err
            }

            msg := types.NewMsgBuyName(args[0], coins, cliCtx.GetFromAddress())
            err = msg.ValidateBasic()
            if err != nil {
                return err
            }

            return utils.GenerateOrBroadcastMsgs(cliCtx, txBldr, []sdk.Msg{msg})
        },
    }
}

// GetCmdSetName is the CLI command for sending a SetName transaction
func GetCmdSetName(cdc *codec.Codec) *cobra.Command {
    return &cobra.Command{
        Use:   "set-name [name] [value]",
        Short: "set the value associated with a name that you own",
        Args:  cobra.ExactArgs(2),
        RunE: func(cmd *cobra.Command, args []string) error {
            cliCtx := context.NewCLIContext().WithCodec(cdc)
            inBuf := bufio.NewReader(cmd.InOrStdin())
            txBldr := auth.NewTxBuilderFromCLI(inBuf).WithTxEncoder(utils.GetTxEncoder(cdc))

            // if err := cliCtx.EnsureAccountExists(); err != nil {
            //     return err
            // }

            msg := types.NewMsgSetName(args[0], args[1], cliCtx.GetFromAddress())
            err := msg.ValidateBasic()
            if err != nil {
                return err
            }

            // return utils.CompleteAndBroadcastTxCLI(txBldr, cliCtx, msgs)
            return utils.GenerateOrBroadcastMsgs(cliCtx, txBldr, []sdk.Msg{msg})
        },
    }
}

// GetCmdDeleteName is the CLI command for sending a DeleteName transaction
func GetCmdDeleteName(cdc *codec.Codec) *cobra.Command {
    return &cobra.Command{
        Use:   "delete-name [name]",
        Short: "delete the name that you own along with it's associated fields",
        Args:  cobra.ExactArgs(1),
        RunE: func(cmd *cobra.Command, args []string) error {
            cliCtx := context.NewCLIContext().WithCodec(cdc)
            inBuf := bufio.NewReader(cmd.InOrStdin())
            txBldr := auth.NewTxBuilderFromCLI(inBuf).WithTxEncoder(utils.GetTxEncoder(cdc))

            msg := types.NewMsgDeleteName(args[0], cliCtx.GetFromAddress())
            err := msg.ValidateBasic()
            if err != nil {
                return err
            }

            // return utils.CompleteAndBroadcastTxCLI(txBldr, cliCtx, msgs)
            return utils.GenerateOrBroadcastMsgs(cliCtx, txBldr, []sdk.Msg{msg})
        },
    }
}
```

Notes on the above code:

* The `authcmd` package is used here. [The godocs have more information on usage](https://godoc.org/github.com/cosmos/cosmos-sdk/x/auth/client/cli#GetAccountDecoder). It provides access to accounts controlled by the CLI and facilitates signing.

#### Now your ready to define the routes that the REST client will use to communicate with your module. <a id="now-your-ready-to-define-the-routes-that-the-rest-client-will-use-to-communicate-with-your-module"></a>

If you had any difficulties following this tutorial or simply want to discuss Cosmos tech with us you can [**join our community today**](https://discord.gg/fszyM7K)!

