<!--

@license Apache-2.0

Copyright (c) 2025 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# gger

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Perform the rank 1 operation `A = α*x*y^T + A`.



<section class="usage">

## Usage

```javascript
import gger from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-gger@deno/mod.js';
```

You can also import the following named exports from the package:

```javascript
import { ndarray } from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-gger@deno/mod.js';
```

#### gger( order, M, N, α, x, sx, y, sy, A, lda )

Performs the rank 1 operation `A = α*x*y^T + A`, where `α` is a scalar, `x` is an `M` element vector, `y` is an `N` element vector, and `A` is an `M` by `N` matrix.

```javascript
var A = [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ];
var x = [ 1.0, 1.0 ];
var y = [ 1.0, 1.0, 1.0 ];

gger( 'row-major', 2, 3, 1.0, x, 1, y, 1, A, 3 );
// A => [ 2.0, 3.0, 4.0, 5.0, 6.0, 7.0 ]
```

The function has the following parameters:

-   **order**: storage layout.
-   **M**: number of rows in the matrix `A`.
-   **N**: number of columns in the matrix `A`.
-   **α**: scalar constant.
-   **x**: an `M` element input array.
-   **sx**: stride length for `x`.
-   **y**: an `N` element input array.
-   **sy**: stride length for `y`.
-   **A**: input matrix stored in linear memory.
-   **lda**: stride of the first dimension of `A` (a.k.a., leading dimension of the matrix `A`).

The stride parameters determine which elements in the strided arrays are accessed at runtime. For example, to iterate over every other element in `x` and `y`,

```javascript
var A = [ 1.0, 4.0, 2.0, 5.0, 3.0, 6.0 ];
var x = [ 1.0, 0.0, 1.0, 0.0 ];
var y = [ 1.0, 0.0, 1.0, 0.0, 1.0, 0.0 ];

gger( 'column-major', 2, 3, 1.0, x, 2, y, 2, A, 2 );
// A => [ 2.0, 5.0, 3.0, 6.0, 4.0, 7.0 ]
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

// Initial arrays...
var x0 = new Float64Array( [ 0.0, 1.0, 1.0 ] );
var y0 = new Float64Array( [ 0.0, 1.0, 1.0, 1.0 ] );
var A = new Float64Array( [ 1.0, 4.0, 2.0, 5.0, 3.0, 6.0 ] );

// Create offset views...
var x1 = new Float64Array( x0.buffer, x0.BYTES_PER_ELEMENT*1 ); // start at 2nd element
var y1 = new Float64Array( y0.buffer, y0.BYTES_PER_ELEMENT*1 ); // start at 2nd element

gger( 'column-major', 2, 3, 1.0, x1, -1, y1, -1, A, 2 );
// A => <Float64Array>[ 2.0, 5.0, 3.0, 6.0, 4.0, 7.0 ]
```

#### gger.ndarray( M, N, α, x, sx, ox, y, sy, oy, A, sa1, sa2, oa )

Performs the rank 1 operation `A = α*x*y^T + A`, using alternative indexing semantics and where `α` is a scalar, `x` is an `M` element vector, `y` is an `N` element vector, and `A` is an `M` by `N` matrix.

```javascript
var A = [ 1.0, 2.0, 3.0, 4.0, 5.0, 6.0 ];
var x = [ 1.0, 1.0 ];
var y = [ 1.0, 1.0, 1.0 ];

gger.ndarray( 2, 3, 1.0, x, 1, 0, y, 1, 0, A, 3, 1, 0 );
// A => [ 2.0, 3.0, 4.0, 5.0, 6.0, 7.0 ]
```

The function has the following additional parameters:

-   **sa1**: stride of the first dimension of `A`.
-   **sa2**: stride of the second dimension of `A`.
-   **oa**: starting index for `A`.
-   **ox**: starting index for `x`.
-   **oy**: starting index for `y`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying buffer, the offset parameters support indexing semantics based on starting indices. For example,

```javascript
import Float64Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float64@deno/mod.js';

var A = [ 0.0, 0.0, 1.0, 4.0, 2.0, 5.0, 3.0, 6.0 ];
var x = [ 0.0, 1.0, 0.0, 1.0, 0.0 ];
var y = [ 0.0, 1.0, 0.0, 1.0, 0.0, 1.0, 0.0 ];

gger.ndarray( 2, 3, 1.0, x, 2, 1, y, 2, 1, A, 1, 2, 2 );
// A => [ 0.0, 0.0, 2.0, 5.0, 3.0, 6.0, 4.0, 7.0 ]
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   `gger()` corresponds to the [BLAS][blas] level 2 function [`dger`][dger] with the exception that this implementation works with any array type, not just Float64Arrays. Depending on the environment, the typed versions ([`dger`][@stdlib/blas/base/dger], [`sger`][@stdlib/blas/base/sger], etc.) are likely to be significantly more performant.
-   Both functions support array-like objects having getter and setter accessors for array element access (e.g., [`@stdlib/array-base/accessor`][@stdlib/array/base/accessor]).

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
import discreteUniform from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-array-discrete-uniform@deno/mod.js';
import gger from 'https://cdn.jsdelivr.net/gh/stdlib-js/blas-base-gger@deno/mod.js';

var opts = {
    'dtype': 'generic'
};

var M = 3;
var N = 5;

var A = discreteUniform( M*N, 0, 255, opts );
var x = discreteUniform( M, 0, 255, opts );
var y = discreteUniform( N, 0, 255, opts );

gger( 'row-major', M, N, 1.0, x, 1, y, 1, A, N );
console.log( A );

gger.ndarray( M, N, 1.0, x, 1, 0, y, 1, 0, A, 1, M, 0 );
console.log( A );
```

</section>

<!-- /.examples -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2026. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/blas-base-gger.svg
[npm-url]: https://npmjs.org/package/@stdlib/blas-base-gger

[test-image]: https://github.com/stdlib-js/blas-base-gger/actions/workflows/test.yml/badge.svg?branch=v0.1.0
[test-url]: https://github.com/stdlib-js/blas-base-gger/actions/workflows/test.yml?query=branch:v0.1.0

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/blas-base-gger/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/blas-base-gger?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/blas-base-gger.svg
[dependencies-url]: https://david-dm.org/stdlib-js/blas-base-gger/main

-->

[chat-image]: https://img.shields.io/badge/zulip-join_chat-brightgreen.svg
[chat-url]: https://stdlib.zulipchat.com

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/blas-base-gger/tree/deno
[deno-readme]: https://github.com/stdlib-js/blas-base-gger/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/blas-base-gger/tree/umd
[umd-readme]: https://github.com/stdlib-js/blas-base-gger/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/blas-base-gger/tree/esm
[esm-readme]: https://github.com/stdlib-js/blas-base-gger/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/blas-base-gger/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/blas-base-gger/main/LICENSE

[blas]: http://www.netlib.org/blas

[dger]: https://netlib.org/lapack/explore-html/d8/d75/group__ger_gaef5d248da0fdfb62bccb259725935cb8.html

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@stdlib/blas/base/dger]: https://github.com/stdlib-js/blas-base-dger/tree/deno

[@stdlib/blas/base/sger]: https://github.com/stdlib-js/blas-base-sger/tree/deno

[@stdlib/array/base/accessor]: https://github.com/stdlib-js/array-base-accessor/tree/deno

<!-- <related-links> -->

<!-- </related-links> -->

</section>

<!-- /.links -->
