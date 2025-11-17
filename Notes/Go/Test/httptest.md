#GoTest #Golang #IMP_PACKAGE 
# Go HTTP Testing with `httptest`

The `net/http/httptest` package is a core part of the Go standard library, providing essential utilities for unit testing your HTTP-based Go code.

It allows you to test your HTTP handlers (servers) and HTTP clients in isolation, without needing to bind to a real network port or make external network calls. This makes your tests fast, reliable, and self-contained.

This README covers the two primary use cases for the package:

1. **Testing HTTP Handlers (Servers)** with `httptest.NewRecorder`
    
2. **Testing HTTP Clients** with `httptest.NewServer`
    

## 1. Testing HTTP Handlers with `httptest.NewRecorder`

This is the most common use case. You have an `http.Handler` or `http.HandlerFunc` (your server-side logic), and you want to test if it behaves correctly given a specific request.

The `httptest.NewRecorder` is a mock implementation of `http.ResponseWriter` that "records" everything your handler writes to it—the status code, the response body, and any headers.

### How it Works

1. You create a `http.Request` object to simulate an incoming request (e.g., `GET /hello?name=Go`).
    
2. You create a `httptest.NewRecorder`.
    
3. You call your handler's `ServeHTTP` method directly, passing it the mock request and the recorder.
    
4. Your handler runs, and instead of writing to a network connection, it writes its response to the recorder.
    
5. You then inspect the fields of the recorder (`rr.Code`, `rr.Body.String()`, `rr.Header()`) to assert that your handler produced the correct response.
    

### Example: Testing a "Hello" Handler

Let's say you have this handler in `main.go`:

```
package main

import (
	"fmt"
	"net/http"
)

// HelloHandler greets a user.
func HelloHandler(w http.ResponseWriter, r *http.Request) {
	name := r.URL.Query().Get("name")
	if name == "" {
		name = "Guest"
	}

	w.WriteHeader(http.StatusOK) // Set HTTP 200
	fmt.Fprintf(w, "Hello, %s!", name)
}
```

Your test file, `main_test.go`, would look like this:

```
package main

import (
	"net/http"
	"net/http/httptest"
	"testing"
)

func TestHelloHandler(t *testing.T) {
	// 1. Create the mock request
	// We are simulating a "GET /hello?name=Go" request
	req := httptest.NewRequest(http.MethodGet, "/hello?name=Go", nil)

	// 2. Create the ResponseRecorder
	rr := httptest.NewRecorder()

	// 3. Call the handler directly
	// Note: We use http.HandlerFunc to adapt our HelloHandler
	handler := http.HandlerFunc(HelloHandler)
	handler.ServeHTTP(rr, req)

	// 4. Assert the results
	
	// Check the status code
	if status := rr.Code; status != http.StatusOK {
		t.Errorf("handler returned wrong status code: got %v want %v",
			status, http.StatusOK)
	}

	// Check the response body
	expected := "Hello, Go!"
	if rr.Body.String() != expected {
		t.Errorf("handler returned unexpected body: got %v want %v",
			rr.Body.String(), expected)
	}
}

func TestHelloHandler_Guest(t *testing.T) {
	// Test the fallback "Guest" case
	req := httptest.NewRequest(http.MethodGet, "/hello", nil)
	rr := httptest.NewRecorder()

	handler := http.HandlerFunc(HelloHandler)
	handler.ServeHTTP(rr, req)

	if rr.Code != http.StatusOK {
		t.Errorf("handler returned wrong status code: got %v want %v",
			rr.Code, http.StatusOK)
	}

	expected := "Hello, Guest!"
	if rr.Body.String() != expected {
		t.Errorf("handler returned unexpected body: got %v want %v",
			rr.Body.String(), expected)
	}
}
```

## 2. Testing HTTP Clients with `httptest.NewServer`

This is for testing the _other_ side: code that _makes_ HTTP requests.

The `httptest.NewServer` starts a real, local HTTP server that listens on a system-chosen, available port. It's designed to be a mock _backend_ for your client code to talk to.

### How it Works

1. You define a simple `http.HandlerFunc` that acts as your mock server, returning whatever fake data you need for your test (e.g., a specific JSON payload or a 404 error).
    
2. You pass this mock handler to `httptest.NewServer`.
    
3. The server starts and provides a `server.URL` field (e.g., `http://127.0.0.1:54321`).
    
4. **Crucially, `defer server.Close()`** to ensure the server is shut down after your test finishes.
    
5. You configure your HTTP client code to make its request to `server.URL` instead of the real-world URL.
    
6. You run your client code and assert that it behaved correctly based on the mock response it received.
    

### Example: Testing a "GitHub Client"

Let's say you have a function that fetches a user's name from a (simplified) GitHub API.

Your client code in `client.go`:

```
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
)

// GetGitHubUserName fetches a user's name from some API.
// We pass in the baseURL so we can swap it for a test server.
func GetGitHubUserName(client *http.Client, baseURL string, username string) (string, error) {
	apiURL := fmt.Sprintf("%s/users/%s", baseURL, username)
	
	resp, err := client.Get(apiURL)
	if err != nil {
		return "", err
	}
	defer resp.Body.Close()

	if resp.StatusCode != http.StatusOK {
		return "", fmt.Errorf("API request failed with status: %d", resp.StatusCode)
	}

	var data struct {
		Name string `json:"name"`
	}

	if err := json.NewDecoder(resp.Body).Decode(&data); err != nil {
		return "", err
	}

	return data.Name, nil
}
```

Your test file, `client_test.go`, would look like this:

```
package main

import (
	"fmt"
	"net/http"
	"net/http/httptest"
	"testing"
)

func TestGetGitHubUserName(t *testing.T) {
	// 1. Define the mock server's handler
	mockAPIHandler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		// Check that the client is requesting the correct path
		if r.URL.Path != "/users/golang" {
			http.Error(w, "Not Found", http.StatusNotFound)
			return
		}

		// Send back a mock JSON response
		w.WriteHeader(http.StatusOK)
		w.Header().Set("Content-Type", "application/json")
		fmt.Fprint(w, `{"id": 123, "login": "golang", "name": "The Go Team"}`)
	})

	// 2. Start the mock server
	server := httptest.NewServer(mockAPIHandler)
	
	// 3. Defer its closure!
	defer server.Close()

	// 4. Configure your client to use the server's URL
	client := http.DefaultClient
	baseURL := server.URL // This is the magic! e.g., "[http://127.0.0.1:12345](http://127.0.0.1:12345)"

	// 5. Run the function under test
	name, err := GetGitHubUserName(client, baseURL, "golang")

	// 6. Assert the results
	if err != nil {
		t.Fatalf("expected no error, got: %v", err)
	}

	expected := "The Go Team"
	if name != expected {
		t.Errorf("expected name %q, got %q", expected, name)
	}
}
```

## Summary: Which One to Use?

- **Use `httptest.NewRecorder` when:**
    
    - You are testing your **own handler** (your server-side code).
        
    - You want to check what **response** your handler _writes_.
        
- **Use `httptest.NewServer` when:**
    
    - You are testing your **HTTP client** (code that _makes_ requests).
        
    - You need to provide a **mock backend** for your client to talk to.