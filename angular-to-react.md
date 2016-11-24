# Migrating from AngularJS 1.8 to React

### Things to keep in mind

- Not freaking out co-workers who feel secure in their established ways
- 

### Advantages

1. Faster
2. Lighter
3. More flexible
4. Lively community
5. Long-term support
6. Better match for our architecture patterns


### Disadvantages

1. No more template caching
2. Learning curve


### Two main ways to do this

1. Full rewrite
    Time consumming; from the user's perspective, nothing moves.
2. Incrementally
    File size; our bundle is already huge -> adding React on top will make
    Confusion; having to reason in two different frameworks

### Considerations

1. Webpack?
    - Hot-module replacement!!
2. Rollup?
    - Tiny bundles!
