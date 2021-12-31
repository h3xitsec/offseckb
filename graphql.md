# notes
## basic enumeration
```json
// show name of all the types being used
{__schema{types{name,fields{name}}}}

// extract all the types, fields and arguments
{__schema{types{name,fields{name, args{name,description,type{name, kind, ofType{name, kind}}}}}}}
```

## database enumeration via introspection
```json
fragment FullType on __Type {
  kind
  name
  description
  fields {
    name
    description
    args {
      ...InputValue
    }
    type {
      ...TypeRef
    }
  }
  inputFields {
    ...InputValue
  }
  interfaces {
    ...TypeRef
  }
  enumValues {
    name
    description
  }
  possibleTypes {
    ...TypeRef
  }
}

fragment InputValue on __InputValue {
  name
  description
  type {
    ...TypeRef
  }
  defaultValue
}

fragment TypeRef on __Type {
  kind
  name
  ofType {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
        ofType {
          kind
          name
          ofType {
            kind
            name
            ofType {
              kind
              name
              ofType {
                kind
                name
              }
            }
          }
        }
      }
    }
  }
}

query IntrospectionQuery {
  __schema {
    queryType {
      name
    }
    mutationType {
      name
    }
    types {
      ...FullType
    }
    directives {
      name
      description
      locations
      args {
        ...InputValue
      }
    }
  }
}
// url encoded
fragment%20FullType%20on%20__Type%20%7B%0A%20%20kind%0A%20%20name%0A%20%20description%0A%20%20fields%20%7B%0A%20%20%20%20name%0A%20%20%20%20description%0A%20%20%20%20args%20%7B%0A%20%20%20%20%20%20...InputValue%0A%20%20%20%20%7D%0A%20%20%20%20type%20%7B%0A%20%20%20%20%20%20...TypeRef%0A%20%20%20%20%7D%0A%20%20%7D%0A%20%20inputFields%20%7B%0A%20%20%20%20...InputValue%0A%20%20%7D%0A%20%20interfaces%20%7B%0A%20%20%20%20...TypeRef%0A%20%20%7D%0A%20%20enumValues%20%7B%0A%20%20%20%20name%0A%20%20%20%20description%0A%20%20%7D%0A%20%20possibleTypes%20%7B%0A%20%20%20%20...TypeRef%0A%20%20%7D%0A%7D%0A%0Afragment%20InputValue%20on%20__InputValue%20%7B%0A%20%20name%0A%20%20description%0A%20%20type%20%7B%0A%20%20%20%20...TypeRef%0A%20%20%7D%0A%20%20defaultValue%0A%7D%0A%0Afragment%20TypeRef%20on%20__Type%20%7B%0A%20%20kind%0A%20%20name%0A%20%20ofType%20%7B%0A%20%20%20%20kind%0A%20%20%20%20name%0A%20%20%20%20ofType%20%7B%0A%20%20%20%20%20%20kind%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20ofType%20%7B%0A%20%20%20%20%20%20%20%20kind%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20ofType%20%7B%0A%20%20%20%20%20%20%20%20%20%20kind%0A%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20ofType%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20kind%0A%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20ofType%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20kind%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20ofType%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20kind%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A%0Aquery%20IntrospectionQuery%20%7B%0A%20%20__schema%20%7B%0A%20%20%20%20queryType%20%7B%0A%20%20%20%20%20%20name%0A%20%20%20%20%7D%0A%20%20%20%20mutationType%20%7B%0A%20%20%20%20%20%20name%0A%20%20%20%20%7D%0A%20%20%20%20types%20%7B%0A%20%20%20%20%20%20...FullType%0A%20%20%20%20%7D%0A%20%20%20%20directives%20%7B%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20description%0A%20%20%20%20%20%20locations%0A%20%20%20%20%20%20args%20%7B%0A%20%20%20%20%20%20%20%20...InputValue%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&variables=null&operationName=IntrospectionQuery
```