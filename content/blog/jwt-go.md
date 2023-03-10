---
title: "Improvements to jwt-go"
date: "2020-05-15"
subtitle: "A proposal to improve jwt-go"
---


# Improving jwt-go

## Brief run-down of jwt

A relatively new technology, jwt are **j**son **w**eb **t**>okens. They are represented as a text string, three parts separated by dots. The three parts are base64-encoded JSON objects that can be decoded and then re-constructed as JSON objects.

Their structure is what makes them special, and it's well-defined in a [>RFC7519](https://tools.ietf.org/html/rfc7519), and I won't go too far into detail, but essentially we have the header, the body, and the signature. The header defines some meta information about the token, the body contains the really useful information, and those two parts are combined and signed, and the result is put into the signature. Tokens can be signed either via symmetric key or asymmetric key, and the key type used is explained in the header. There are also companion standards that cover things such as encrypted tokens (*JOSE*, *JWS*, *JWE*) and also standards for sharing of keys (eg working with JWKS). Suffice to say, a token is **signed**, and it can also be **encrypted**, but the magic is in the signing.

Why is signing important? Because jwt are essentially statements. They're not to be used to track state. We are simply establishing facts. The reserved keywords include terms such as *not before*, *expiry*, *single use*, and *issuer*, which help us establish whether or not to trust these statements (and also the signature, which if we can't validate, we should simply disregard the entire jwt).

Essentially a jwt is trying to communicate: "this is a jwt signed with hs256"."this user claims to have the following permissions"."this is a signature which should validate the aforementioned claims". Whether or not the claims are to be trusted depends on your jwt tooling. They don't have passwords, they don't have two-factor, they're much more primitive than that.

When used in a web API or application, we usually have an `auth` component which will create and sign tokens when a user `POST`s their credentials, and an `authz` module which will validate tokens from a user if they make a request to the server. The tokens don't record any state, all that needs to be handled elsewhere, in contrast to how things are often done with cookies. You can store jwt anywhere -- usually cookies or local storage are the best places.

`jwt-go` allows you to sign and distribute tokens using your own keys, and it also allows you to verify and re-construct token content based on keys. It doesn't do any actual "middleware" or "framework" kind of thing, that's up to you to implement. It does however provide methods to validate-and-reject, along with unsafe methods, depending on the work you're doing. I discovered jwts back in 2015, and started using jwt-go around late 2015, but only really got into the groove of it around mid-2016. In my opinion, what's holding back understanding and adoption is the tooling -- there's really this weird wedge splitting "barebones" libraries like `jwt-go` (it's hardly barebones!) from libraries like `authboss` (which honestly set me back by a lot -- it was overly opionnated and complicated for what should have been a small helpful package).

## Some of the flaws in the original jwt-go<

Let's look at some of the example code in the `jwt-go` package:

{{< highlight go >}}
func ExampleNewWithClaims_standardClaims() {
	mySigningKey := []byte("AllYourBase")

	// Create the Claims
	claims := &jwt.StandardClaims{
		ExpiresAt: 15000,
		Issuer:    "test",
	}

	token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
	ss, err := token.SignedString(mySigningKey)
	fmt.Printf("%v %v", ss, err)
	//Output: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MDAwLCJpc3MiOiJ0ZXN0In0.QsODzZu3lUZMVdhbO76u3Jv02iYCvEHcYVUI1kOWEU0 <nil>
}
{{< /highlight >}}

There's nothing necessarily wrong with this -- but it definitely doesn't feel very go-like. It's perhaps not as obvious here, but if you dive in long enough you will see that there are many functions and methods that don't seem to follow a go-like pattern, or that bend common patterns. In my experience it also ends up being unwieldly when using RSA public/private key pairs as well.

## How my jwt-go library works
{{< highlight go >}}
package main

import "fmt"

func main(){
	claims: &struct {
		Foo string `json:"foo"`
		Bar int    `json:"bar"`
		jwt.StandardClaims
	}{
		Foo: "bar",
		Bar: 5,
	},

	enc, err := jwt.NewHS256Encoder(strings.NewReader(test.secret))
	if err != nil {
		fmt.Printf("can't create Encoder: %s", err)
		panic(err)
	}
	dec, err := jwt.NewHS256Decoder(strings.NewReader(test.secret))
	if err != nil {
		panic("can't create Decoder: %s", err)
	}
	tokenstr, err := enc.Encode(claims)

	if err != nil {
		fmt.Printf("can't Encode: %s", err)
	}

	if err := dec.Decode(tokenstr, cp.(jwt.Claims)); err != nil {
		fmt.Printf("can't Decode: %s", err)
	}

}
{{< /highlight >}}

This new Pattern now follows the more typical encoder/decoder pattern that is used all over the go standard library. This also allows us to define an interface and make all of the encoders/decoders compatible with each other and interchangeable. It's nothing really revolutionary, but I hope it serves you well. I'm in the process of creating all of the encoders and decoders for all the signing methods in jwt-go, and forking the package name as well.

## Acknowledgements
I wrote this code while working for Comcast, and was able to open source it. I also had help from [Olivier Gagnon](https://github.com/hydroflame). Hopefully it serves you well. Thanks Comcast!</p> 
