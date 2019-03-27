
<!-- README.md is generated from README.Rmd. Please edit that file -->

# sealr

[![Build
Status](https://travis-ci.org/jandix/sealr.svg?branch=master)](https://travis-ci.org/jandix/sealr)
[![codecov](https://codecov.io/gh/jandix/sealr/branch/master/graph/badge.svg)](https://codecov.io/gh/jandix/sealr)

The goal of sealr is to provide multiple authentication and
authorization strategies for [plumber](https://www.rplumber.io/) by
using
[filters](https://www.rplumber.io/docs/routing-and-input.html#filters).
In doing so, we hope to make best practices in authentication easy to
implement for the R community. The package is inspired by the amazing
[passport.js](http://www.passportjs.org/) library for Node.js.

## Plumber filters for authentication / authorization

coming soon

## Implementation overview

#### `authenticate`

`sealr`’s main function is the `authenticate` function. `authenticate`
takes a `is_authed_*` function (see below) as input and depending on the
output of this “checker” function, takes action:

  - if the request is authenticated / authorized, it forwards to the
    next plumber handler using `plumber::forward`.
  - if the request is not authenticated / authorized, it `return`s to
    the user, passing forward HTTP status code, description and message
    from the output of the `is_authed_` function.

By accepting a function object as argument, `authenticate` is quite
flexible: You can even pass your own `is_authed` function.

#### `is_authed` family of functions

The functions starting with `is_authed` provide the actual
implementations of the different authentication / authorization
strategies that `sealr` aims to provide. Currently implemented are:

  - `is_authed_jwt`: implements JSON Web Token verification and
    checking.
  - `is_authed_oauth2_google`: implements Google’s OpenID Connect (which
    is based on OAuth2.0)

`is_authed_*` functions return a list with the following elements:

  - `is_authed`: TRUE or FALSE. Result of the check whether the request
    is authenticated / authorized.
  - `status`: character. Optional (typically only set if `is_authed` is
    FALSE). Short description of HTTP status code.
  - `code`: integer. Optional (typically only set if `is_authed` is
    FALSE). HTTP status code.
  - `message`: character. Optional (typically only set if `is_authed` is
    FALSE). Longer description.

Currently, the `plumber` package does not support imposing filters on
individual endpoints (but it is on the developer team’s radar, see [this
issue](https://github.com/trestletech/plumber/issues/108) :)). That
makes it difficult to require different levels of authorization,
e.g. specific authorization for specific endpoints using the plumber
filter method (see
[jwt\_claims\_example](https://github.com/jandix/sealr/tree/master/examples/jwt/jwt_claims_example)).

As a workaround, you can put your authentication / authorization checks
in the individual endpoints. The `is_authed_*` functions are designed so
that you can use them independently from plumber filters as long as you
pass a plumber request and response object.

## Installation

Currently, the package is under development. Please feel free to
contribute to the package. You can install and use the package using
`devtools`.

``` r
devtools::install_github("jandix/sealr")
```

## Contribute

We are still at the very beginning of the package and we welcome any
support and contribution. Below you find a list with possible
authentication strategies that you could implement. The list is not
complete and can be expanded with your suggestions.

#### Possible Strategies

  - \[ \] Bearer Token
  - \[ \] Sessions
  - \[ \] Twitter OAuth
  - \[ \] Facebook OAuth
  - \[ \] Google OAuth

## Testing

You can use curl for testing purposes. Unfortunately, curl quickly gets
quite complicated if you want to add a body, parameters and unique
headers. Therefore, we recommend to use
[Postman](https://www.getpostman.com/) for larger, more complicated
projects.

## Examples

We provide some simple sample implementations for different strategies
and use cases. You can find them in the `examples` folder.

## Warranity Notice

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS
OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
