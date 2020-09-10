Running
=======

* Starting DAML

  ```bash
  daml start --open-browser no
  ```

* Starting the 4 triggers (each on their own terminal)

  ```bash
  daml trigger --dar .daml/dist/fizzbuzz-trigger-0.0.1.dar --trigger-name FizzBuzz:createClassifyTrigger --ledger-host localhost --ledger-port 6865 --ledger-party Manager
  ```

  ```
  daml trigger --dar .daml/dist/fizzbuzz-trigger-0.0.1.dar --trigger-name FizzBuzz:createResultFizzTrigger --ledger-host localhost --ledger-port 6865 --ledger-party Fizz
  ```

  ```
  daml trigger --dar .daml/dist/fizzbuzz-trigger-0.0.1.dar --trigger-name FizzBuzz:createResultBuzzTrigger --ledger-host localhost --ledger-port 6865 --ledger-party Buzz

  ```

  ```
  daml trigger --dar .daml/dist/fizzbuzz-trigger-0.0.1.dar --trigger-name FizzBuzz:createFizzBuzzValueTrigger --ledger-host localhost --ledger-port 6865 --ledger-party Manager
  ```

* Starting the REPL

  ```
  daml repl --ledger-host=localhost --ledger-port=6865 .daml/dist/fizzbuzz-trigger-0.0.1.dar --import fizzbuzz-trigger
  ```

  * in the REPL, initialize variables

    ```
    import DA.List
    import DA.Optional
    let creator = fromSome (partyFromText "Creator")
    let manager = fromSome (partyFromText "Manager")
    let fizz = fromSome (partyFromText "Fizz")
    let buzz = fromSome (partyFromText "Buzz")
    ```

  * register the classifiers

    ```
    submit fizz (createCmd (Classifier fizz manager))
    submit buzz (createCmd (Classifier buzz manager))
    ```

  * create a few values to process

    ```
    forA [10,11,12,13,14,15] (\ (v) -> submit creator (createCmd (Value v creator manager)))
    ```

  * check the generated fizzbuzz values
  
    ```
    fb <- query @FizzBuzzValue creator
    map (\ (f) -> (f._2.value.value, f._2.fizzbuzz)) fb
    ```