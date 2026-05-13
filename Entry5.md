> 🌄 DevJournal — Day 5 Task: “Bug Hunter’s Bounty” 🐛💰
>
> Mission Brief:
> Today’s quest is all about sharpening your debugging instincts — a skill every great dev wields like a legendary sword. ⚔️
>
> 🧭 Task:
> Pick a small open-source project on GitHub (or one of your own past projects) and:
>
> Find one small bug — maybe a console error, broken layout, or unexpected behavior.
>
> Diagnose it — figure out why it’s happening.
>
> Fix it — patch it up and test your solution.
>
> If you can’t find a bug, intentionally create one (like a typo in logic or a missing semicolon) and then walk through debugging it using logs, breakpoints, or error messages.
>
> 🎯 Goal:
Practice real-world debugging. This hones problem-solving skills that every hiring manager loves to see.
>
> 💡 Bonus XP:
>
> Commit your fix to a branch and write a clear commit message (fix: resolve unexpected crash on load).
>
> Document the bug and solution in a short note — perfect for your portfolio or LinkedIn update.
>
> Bug squashed, XP gained. 🧙‍♂️ Your “Code Sorcery” stat just leveled up!
---
# Response

So because Task 4 ended up taking a bit longer that anticipated, I decided to use the same project for this task too.  So, I am gonna talk about one of my major bugs/errors that I eventually worked out on this project.  

This was one of the main errors I struggled with, 

~~~
Unhandled exception. System.Text.Json.JsonException: The JSON value could not be converted to GeoModel. Path: $ | LineNumber: 0 | BytePositionInLine: 1.
   at System.Text.Json.ThrowHelper.ThrowJsonException_DeserializeUnableToConvertValue(Type propertyType)
   at System.Text.Json.Serialization.Converters.ObjectDefaultConverter`1.OnTryRead(Utf8JsonReader& reader, Type typeToConvert, JsonSerializerOptions options, ReadStack& state, T& value)
   at System.Text.Json.Serialization.JsonConverter`1.TryRead(Utf8JsonReader& reader, Type typeToConvert, JsonSerializerOptions options, ReadStack& state, T& value, Boolean& isPopulatedValue)
   at System.Text.Json.Serialization.JsonConverter`1.ReadCore(Utf8JsonReader& reader, JsonSerializerOptions options, ReadStack& state)
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo`1.ContinueDeserialize(ReadBufferState& bufferState, JsonReaderState& jsonReaderState, ReadStack& readStack)
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo`1.DeserializeAsync(Stream utf8Json, CancellationToken cancellationToken)
   at System.Net.Http.Json.HttpClientJsonExtensions.<FromJsonAsyncCore>g__Core|12_0[TValue,TJsonOptions](HttpClient client, Task`1 responseTask, Boolean usingResponseHeadersRead, CancellationTokenSource linkedCTS, Func`4 deserializeMethod, TJsonOptions jsonOptions, CancellationToken cancellationToken)
   at WeatherService.GetGeoAsync(String cityName, String stateCode, String countryCode) in /workspaces/active-dev/Projects/api-task4/WeatherService.cs:line 26
   at WeatherService.GetWeatherAsync(String cityName, String stateCode, String countryCode) in /workspaces/active-dev/Projects/api-task4/WeatherService.cs:line 12
   at Program.Main() in /workspaces/active-dev/Projects/api-task4/Program.cs:line 10
   at Program.<Main>()
~~~

As obvious as it was to exactly what was happening, I struggled to actually find the culprit. I knew that it had to be an issue with deserializing the JSON, as the lines that it kept referring to in the errors were the API calls.  After a bit longer than I care to admit, I eventually found my issue.  It ended up being a simple JSON formatting error.  I hadn't noticed that on one of the nested objects, it actually returned an array instead on a single object.  Once I was able to reconfigure my code, like shown below, I had it working it no time. 

Such a small change fixed the entire project:

### Before:
```csharp
public async Task<GeoModel> GetGeoAsync(string cityName, string stateCode, string countryCode = "US")
    {
        // create API string url
        string url = "https://api.openweathermap.org/geo/1.0/direct?q=" + $"{cityName},{stateCode},{countryCode}&appid={_appId}";

        // perform API call using created url, deserializes data, stores in local variable
        var response = await _httpClient.GetFromJsonAsync<GeoModel>(url);  // MISTAKE WAS HERE

        // return local variable, only return first object
        return response ?? throw new InvalidOperationException("Failed to deserialize data.");
    }
```
### After:
```csharp
public async Task<GeoModel> GetGeoAsync(string cityName, string stateCode, string countryCode = "US")
    {
        // create API string url
        string url = "https://api.openweathermap.org/geo/1.0/direct?q=" + $"{cityName},{stateCode},{countryCode}&appid={_appId}";

        // perform API call using created url, deserializes data, stores in local variable
        var response = await _httpClient.GetFromJsonAsync<List<GeoModel>>(url);

        // data deserializes into a List of GeoModels, fails if null or count is 0
        if (response is null || response.Count == 0)
            throw new InvalidOperationException("Failed to deserialize data.");

        // return local variable, only return first object
        return response[0];
    }
```