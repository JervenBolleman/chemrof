# Chem: Schema for chemistry data models and ontologies

GitHub: [chemkg/chemrof](https://github.com/chemkg/chemrof)

This repo includes schema/datamodel for a chemical entity
datamodeling. It also provides an example database and an example of
how to translate this into an OBO-style OWL ontology

## Core Model

Instances of the classes in the data model are CHEBI classes, e.g.

 - Elements: nickel atom, carbon atom, ...
 - Ions: nickel anion, copper(2+), ...
 - Isotopes: carbon-14, carbon-12, ...
 - Molecules:
    - Enantiomers: L-serine
    - Acids: nitrate, ...
    - Drugs: imatinib, ...
    - Racemic Mixtures: thalidomide, ...

## Schema

The schema and overall framework uses [LinkML](https://linkml.github.io)

 * Browse the schema here: [http://chemkg.github.io/chemrof](http://chemkg.github.io/chemrof)
 * See the yaml source in the [src/schema](https://github.com/chemkg/chemrof/blob/master/src/schema) directory

### Schema Organization

The main division is between:

 * [ChemicalEntity](/ChemicalEntity/) - essentially things you can write a formula/inchi for, and
 * [GroupingClass](/GroupingClass/) - generalizations/sets of chemical entities

This distinction may be hard to grasp if you are coming from an OBO
realist ontologist philosophical perspective, but this in fact allows
us to serve a much wider variety of use cases than a strict OBO-style
approach. At the same time we can generate an OBO-style ontology from this representation

### Example

Enantiomers are mirror-image stereoisomers.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Milchs%C3%A4ure_Enantiomerenpaar.svg/298px-Milchs%C3%A4ure_Enantiomerenpaar.svg.png)

The [Enantiomer class](/Enantiomer) represents a chemical entity that has a form (e.g L/D) specified. In the yaml:

```yaml
  enantiomer:
    is_a: stereoisomer
    description: >-
      one of two stereoisomers of a chiral molecule that are mirror images. Example: R-thalidomide
    slot_usage:
      relative configuration:
        range: relative_configuration_enum
      optical configuration:
        range: optical_configuration_enum
      absolute configuration:
        range: absolute_configuration_enum
      enantiomer form of:
        range: molecule
        is_a: owl subclass of
        description: >-
          Example:  R-thalidomide is the enantiomer form of 2-(2,6-dioxopiperidin-3-yl)-1H-isoindole-1,3(2H)-dione
    compound_keys:
      main_key:
        - enantiomer form of
        - absolute configuration
      inchi_based_unique_key:
        - enantiomer form of
        - inchi tetrahedral stereochemical sublayer
    exact_mappings:
      - SIO:010777
```

Note that a class "Enantiomer" does not really make sense at an OBO level where all chemical entities are classes; it is a metaclass.

## Derived artefacts

Currently the main derived artefacts of interest are:

 - [JSON Schema](https://github.com/chemkg/chemrof/tree/master/jsonschema)
 - [ShEx](https://github.com/chemkg/chemrof/tree/master/shex)
 - [Python dataclasses](https://github.com/chemkg/chemrof/tree/master/python)

With care, you also see:

 - [OWL](https://github.com/chemkg/chemrof/tree/master/owl) -- note this should be read as a meta-ontology by OBO people

## Comparison with CHEBI and Wikidata

TODO