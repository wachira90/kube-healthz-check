# kube-healthz-check-
kubernetes  healthz check code example

## golang

```go
http.HandleFunc("/healthz", func(w http.ResponseWriter, r *http.Request) {
    duration := time.Now().Sub(started)
    if duration.Seconds() > 10 {
        w.WriteHeader(500)
        w.Write([]byte(fmt.Sprintf("error: %v", duration.Seconds())))
    } else {
        w.WriteHeader(200)
        w.Write([]byte("ok"))
    }
})
```

## java spring-boot

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.time.Duration;
import java.time.Instant;

@RestController
@RequestMapping("/healthz")
public class HealthCheckController {

    private Instant started = Instant.now();

    @GetMapping
    public ResponseEntity<String> healthCheck() {
        Duration duration = Duration.between(started, Instant.now());
        
        if (duration.getSeconds() > 10) {
            return new ResponseEntity<>("error: " + duration.getSeconds(), HttpStatus.INTERNAL_SERVER_ERROR);
        } else {
            return new ResponseEntity<>("ok", HttpStatus.OK);
        }
    }
}
```

## python

```py
#!python
from flask import Flask
from datetime import datetime

app = Flask(__name__)

started = datetime.now()

@app.route('/healthz')
def healthz():
    duration = datetime.now() - started
    if duration.total_seconds() > 10:
        return "error: {}".format(duration.total_seconds()), 500
    else:
        return "ok", 200

if __name__ == '__main__':
    app.run(port=8080)

```

## .NET Core

```
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.DependencyInjection;
using System;

public class Startup
{
    private DateTime started = DateTime.Now;
    public void ConfigureServices(IServiceCollection services)
    {
        // Add any necessary services here
    }
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.Map("/healthz", HandleHealthz);

        // Additional configurations can be added here
    }

    private void HandleHealthz(IApplicationBuilder app)
    {
        app.Run(async (context) =>
        {
            TimeSpan duration = DateTime.Now - started;
            if (duration.TotalSeconds > 10)
            {
                context.Response.StatusCode = 500;
                await context.Response.WriteAsync($"error: {duration.TotalSeconds}");
            }
            else
            {
                context.Response.StatusCode = 200;
                await context.Response.WriteAsync("ok");
            }
        });
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseKestrel()
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}

```

## php

```php
<?php

$started = microtime(true);

$server = new \swoole_http_server("127.0.0.1", 9501);

$server->on('request', function ($request, $response) use ($started) {
    $duration = microtime(true) - $started;
    if ($duration > 10) {
        $response->status(500);
        $response->end("error: " . $duration);
    } else {
        $response->status(200);
        $response->end("ok");
    }
});

$server->start();
```
