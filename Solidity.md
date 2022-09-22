### From Open Zeppelin guidelines:

Any exception or additions specific to our project are documented below.

- Try to avoid acronyms and abbreviations.
- All state variables should be private.
- Private state variables should have an underscore prefix.

  ```solidity
  contract TestContract {
    uint256 private _privateVar;
    uint256 internal _internalVar;
  }
  ```

- Parameters must not be prefixed with an underscore.

  ```
  function test(uint256 testParameter1, uint256 testParameter2) {
  ...
  }

  ```

- Internal and private functions should have an underscore prefix.

  ```
  function _testInternal() internal {
    ...
  }

  ```

  ```
  function _testPrivate() private {
    ...
  }

  ```

- Events should be emitted immediately after the state change that they
  represent, and consequently they should be named in past tense.

  ````
  function \_burn(address who, uint256 value) internal {
  super.\_burn(who, value);
  emit TokensBurned(who, value);
  }

      ```

      Some standards (e.g. ERC20) use present tense, and in those cases the
      standard specification prevails.

  ````

- Interface names should have a capital I prefix.

  ```
  interface IERC777 {

  ```

  Additionally:

  - Function and variable naming should be unique and explicit.
    For example, avoid naming one variable a word like `option`, and then naming more variables very similar to it like `options` . This way, we can avoid mistakenly using the wrong variable. It's better to be explicit like: `optionToken` and `allOptionAddresses`.
