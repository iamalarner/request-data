
#request-data

## getRequestData(maxSize[optional], raw[optional], callback)

wraps any fn(request, response) function, parses the requests body, and calls the wrapped function with data as the last parameter.

### Normal usage:

    "/myRoute":getRequestData(function(request, response, data){

    });

### With a named handler:

    //handler
    function handleThing(request, response, data){

    }

handleThing will be passed the arguments of data, request, response

    "/myRoute":getRequestData(handleThing);

if you want to use it with a custom router that adds params after request/response, it will still work.

    // beelin syntax

    "/myRoute/`things`":getRequestData(function(request, response, tokens, values, data){

    });

By default request-data will kill a request if you try and send it more than 1e6 bytes.

This can be overriden per handler:

    getRequestData(1e10, function(request, response, data){...

Or globally:

    getRequestData.maxRequestSize = 1e10;

If data fails to parse during JSON.parse, the parse error is passed to the handler instead of data

You can handle this case by checking if data is an error:

    "/myRoute":getRequestData(function(request, response, data){

        if(data instanceof Error){
            // Bad request
            response.statusCode = 400;
            response.end();
            return;
        }

        // Otherwise, data was valid, and has been parsed.

    });