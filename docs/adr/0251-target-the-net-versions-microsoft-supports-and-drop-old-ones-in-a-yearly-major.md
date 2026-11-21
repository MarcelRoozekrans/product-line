# ADR-251: Target the .NET versions Microsoft supports, and drop old ones in a yearly major

## Status

accepted (2026-11-21). Settles a conflict between products.

## Context

Products in the line target different .NET versions. Read from the repositories on 2026-11-21: ZeroAlloc.Telemetry targets net8.0, net9.0, net10.0 and net11.0; AI.Sentinel and Thalos.NET net8.0, net10.0 and net11.0; Rag.NET, Minos.NET and Mnemosyne.NET net10.0 and net11.0; Ariadne.NET and MagicLink.NET net10.0 only. Other ZeroAlloc libraries differ again.

.NET 8 and .NET 9 reached end of support on 2026-11-10. .NET 10 (LTS) and .NET 11 (STS) are supported until 2028-11-14.

SemVer does not say whether dropping a target framework is a breaking change; it defines a major version by incompatible changes to the public API. The .NET runtime team treats dropping a framework from a package as a breaking change: users on that framework can no longer update.

## Decision

* Libraries target every .NET version Microsoft still supports. Today: net10.0 and net11.0.
* Source generators and analyzers target netstandard2.0, as the compiler requires.
* Applications and tools (such as MagicLink.NET, Ariadne.NET's CLI, Themis.NET) target the newest LTS only. Today: net10.0.
* A new .NET version is added once it is officially released.
* A version that has reached end of support is dropped in a major release, once a year, shortly after Microsoft's November release. Other breaking changes of that product are bundled into the same major where possible.

## Consequences

* Libraries that still target net8.0 or net9.0 (ZeroAlloc.Telemetry, AI.Sentinel, Thalos.NET and several ZeroAlloc libraries) drop them in their next major.
* CI no longer needs the .NET 8 and .NET 9 runtimes once those majors ship.
* Users on an unsupported .NET version keep the last major that supported it, but get no new versions.
