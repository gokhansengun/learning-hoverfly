# Hoverfly Learning Notes

1. with `mode capture` API requests are intercepted

1. Hoverfly will overwrite duplicate requests however `stateful` disables this to be able to capture and replay POST requests in the received order

1. with `mode spy`, it will respond with the recorded data, if no match found, it will be returning from the external source

1. with `mode synthesize` response is not looked from the stored simulation data, instead passed to user-supplied executable file

1. with `mode modify`, being similar to Capture mode, requests and responses are not saved. Hoverfly will pass each request to a middleware before forwarding to the destination (external service). Likewise responses are also passed to middleware before being returned to the client.

1. with `mode diff`, Hoverfly forwards the requests to external service and compares the response with the current simulation. The differences may be queried from Hoverfly.

1. default `matching strategy` is `strongest`. it could be explicitly specified with `hoverctl mode simulate --matching-strategy=strongest`. To return `first match` instead of the `strongest match`, use `hoverctl mode simulate --matching-strategy=first`

1. By default templating is disabled, in order to enable it `templated` field must be set to true in the response of the simulation.

1. Hoverfly contains a map of keys and values. It uses this to store its internal state. Some request matchers can be made to only match when Hoverfly is in a certain state, other matchers can be set to muate Hoverfly's state.

1. [An example of transitioning state -moving and item from basket to complete.]( https://hoverfly.readthedocs.io/en/latest/pages/keyconcepts/state/settingstate.html)

1. Hoverfly may require a state in order to match with `requireState`

1. `hoverctl state xx` could be used for testing purposes.

1. with `sequences`, it is possible to recreate a sequence of different responses for a single request.

    ```json
    "transitionsState" : {
        "sequence:1" : "2",
    }
    ```

1. destination filtering may be enabled with `hoverctl destination <xx>`

1. `middlewares` works differently depending on the Hoverfly's mode

    ```text
    Capture mode: middleware affects only outgoing requests
    Simulate mode: middleware affects only incoming responses (cache contents remain untouched)
    Synthesize mode: middleware creates responses
    Modify mode: middleware affects requests and responses
    ```

1. `local middleware` is able to execute a script or binary file on a host OS. It only requires that the MW is able to receive JSON from stdin and pass JSON to stdout.

1. `http middleware` is same as `local MW` but Hoverfly uses HTTP to post JSON, it expects to get JSON again with HTTP 200

1. In all cases, MW is expected to keep the JSON scheme untouched.

1. Capturing a sequence of responses use

    ```
    hoverctl mode capture --stateful
    ```

1. Delays may be added based on host, http method and URL pattern.

1. By default, request headers are not captured. If you want to capture headers, you will need to specify them when setting capture mode.

    ```
    hoverctl mode capture --headers "User-Agent,Content-Type,Authorization"
    hoverctl mode capture --all-headers
    ```

1. It is possible to filter and export simulation to separate JSON files

    ```
    hoverctl export echo.json --url-pattern "echo.jsontest.com"     // export simulations for echo.jsontest.com only
    hoverctl export api.json --url-pattern "(.+).jsontest.com"      // export simulations for all jsontest.com subdomains
    ```

1. Hoverfly can also import simulation data that is stored on a remote

    ```
    hoverctl import https://example.com/example.json
    ```

1. [Adding a Python middleware script]( https://hoverfly.readthedocs.io/en/latest/pages/tutorials/basic/randomlatency/randomlatency.html)

1. There are two ways in simulating HTTPS APIs. One is to use Hoverfly's default certificate and other is to have it generate a new certificate.

1. Using the default certificate is detailed [here](https://hoverfly.readthedocs.io/en/latest/pages/tutorials/basic/https/https.html)

1. Configuring SSL with custom certificate is detailed [here](https://hoverfly.readthedocs.io/en/latest/pages/tutorials/advanced/configuressl/configuressl.html)
