---
title: MIF Schema

toc_footers:
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# <a name="introduction">Introduction</a>

The Materials Information File (MIF) is a schema that is used to impose structure on materials data, facilitating its processing and transfer. It is designed to be flexible in order to represent the often complex data that is associated with materials research, development, and manufacturing.

Besides those that are listed in this document, objects will continue to be added to the core of the MIF schema. Additionally, users can add data as they see fit as [secondary objects](#extending), which could be promoted to the core schema in the future. The schema should be used as a basis for structuring materials data, with users adding to it as needed.

# <a name="json">Adherence to JSON Standards</a>

MIF's must always be compliant with JSON standards.

Particular to the MIF schema, all field names must be in <a name="camel_case"> camel case</a>: no spaces between words; all letters are lowercase, except the first letter of each word, which is uppercase; the first letter of the name is also lowercase. For example, the name "phase diagram" would be written as "phaseDiagram". Do not use underscores, hyphens, or other non-alphanumeric characters in field names.

# <a name="core">The Core Schema</a>

This section contains the list of MIF objects that are part of the core schema. Each section contains a list of all of the fields that are a part of that object and information about what it contains.

## <a name="schema_sample">Sample</a>

Sample objects are high-level structures used to store information about measurements that were made on a material.

field name | nullable | value type | description
-----------|----------|------------|------------
material | false | single [material](#schema_material) object | A description of the material.
measurement | true | array of [measurement](#schema_measurement) objects | Information about any measurements that were taken on the sample.
reference | true | array of [reference](#schema_reference) objects | Any references where information about the sample is published. If any references are specific to a single measurement, then they should be attached to the corresponding object in the *measurement* field.
contact | true | array of [person](#schema_person) objects | Any people that can be contacted for information about the sample. If one or more people should be contacted only for a particular measurement, then they should be attached to the corresponding object in the *measurement* field.
license | true | array of strings | Information about any licenses that apply to the entire sample. If any license applies only to a particular measurement, then it should be attached to the corresponding object in the *measurement* field.

## <a name="schema_system">System</a>

System objects are high-level objects used to store information about a system of materials and the properties of those materials.

field name | nullable | value type | description
-----------|----------|------------|------------
systemType | true | string | Type of the system. For example, "Ionic liquid" or "Multi-layer solar cell".
component | false | array of [component](#schema_component) objects | List of the individual components that make up the system. The order of the components may be meaningful; for example, when describing a multi-layer solar cell, the sample order should correspond to their order in the physical device.
condition | true | array of [value](#schema_value) objects | Information about conditions of the system as a whole. For example, this could describe methods used to join samples in a multi-layered device.
measurement | true | array of [measurement](#schema_measurement) objects | Information about any measurements that were taken on the system.
reference | true | array of [reference](#schema_reference) objects | Any references where information about the system is published. If any references are specific to a single phase or measurement, then they should be attached to the corresponding object in the *phase* or *measurement* fields.
contact | true | array of [person](#schema_person) objects | Any people that can be contacted for information about the system. If one or more people should be contacted only for a particular sample or measurement, then they should be attached to the corresponding object in the *sample* or *measurement* fields.
license | true | array of strings | Information about any licenses that apply to the entire system. If any license applies only to a particular sample or measurement, then it should be attached to the corresponding object in the *sample* or *measurement* fields.

## <a name="schema_component">Component</a>

Component objects are used inside of [system](#schema_system) objects to store a system component of the system.

field name | nullable | value type | description
-----------|----------|------------|------------
label | true | string | Label to apply to the component. For example, in a system containing a thin film grown on a substrate, this might be equal to "substrate".
sample | false | [sample](#schema_sample) object | Information about the particular sample that makes up this component.

## <a name="schema_phase_diagram">Phase Diagram</a>

Phase diagram objects are high-level objects used to store information about a phase diagram and the materials that it describes.

field name | nullable | value type | description
-----------|----------|------------|------------
vertex | true | array of strings | Labels to apply to each of the vertices of the phase diagram. The order of these labels is consistent with coordinates. For example, ["Cu", "Au"] would correspond to Cu at coordinates (1, 0) and Au at coordinates (0, 1).
phase | true | array of [phase](#schema_phase) objects | Detailed information about materials described in the phase diagram using [sample](#schema_sample) objects (via [phase](#schema_phase) objects). To simply add the name of a material use the *label* field instead.
label | true | array of [point](#schema_point) objects | Labels to apply to the phase diagram. Note that anything already in the *vertex* field should not be included here.
boundary | true | array of [line](#schema_line) objects | Boundaries inside of the phase diagram.
dataType | true | "Experimental" or "Computational" | Label used to mark the data as generated via physical experiment or computational methods.
reference | true | array of [reference](#schema_reference) objects | Any references where information about the phase diagram is published. If any references are specific to a single phase, then they should be attached to the corresponding object in the *phase* field.
contact | true | array of [person](#schema_person) objects | Any people that can be contacted for information about the phase diagram. If one or more people should be contacted only for a particular phase, then they should be attached to the corresponding object in the *phase* field.
license | true | array of strings | Information about any licenses that apply to the entire phase diagram. If any license applies only to a particular phase, then it should be attached to the corresponding object in the *phase* field.

## <a name="schema_material">Material</a>

Material objects store information about the chemical composition of a material and its conditions.

field name | nullable | value type | description
-----------|----------|------------|------------
chemicalFormula | true | string | The chemical formula of the material using IUPAC standards.
commonName | true | array of strings | List of the common name of the material.
composition | true | array of [composition](#schema_composition) objects | Elements and the atomic/weight percent of each in the material.
condition | false | array of [value](#schema_value) objects | Conditions of the material such as its crystallinity, morphology, purity, etc. Note that external conditions generally do not belong in this field, but are more appropriately placed inside, for example, the *condition* field of a [measurement](#schema_measurement) object.

\* While *chemicalFormula*, *commonName*, and *composition* are all nullable, at least one must be non-null.

## <a name="schema_composition">Composition</a>

Composition objects contain information about the amount of a single element in a material.

field name | nullable | value type | description
-----------|----------|------------|------------
element | false | string | The symbol of the element.
weightPercent | true | string | The percentage of the weight of the material that is this element.
atomicPercent | true | string | The percentage of the atoms in the material that are this element.

\* While *weightPercent* and *atomicPercent* are both nullable, at least one must be non-null.

## <a name="schema_measurement">Measurement</a>

Measurement objects contain information about a measurement of a sample and the external conditions that were applied.

field name | nullable | value type | description
-----------|----------|------------|------------
property | false | single [value](#schema_value) object | The name, value, and units of the measured property.
condition | true | array of [value](#schema_value) objects | Details of the external conditions applied during a measurement. For example, the temperature and/or pressure.
method | true | string | Description of the measurement method. For example, this field might contain information about a piece of equipment that was used, or information about a version of code with which a simulation was performed.
dataType | true | "Experimental" or "Computational" | Label used to mark the data as generated via physical experiment or computational methods.
reference | true | array of [reference](#schema_reference) objects | Any references where information about the measurement is published.
contact | true | array of [person](#schema_person) objects | Any people that can be contacted for information about the measurement.
license | true | array of strings | Information about any licenses that apply to the measurement.

## <a name="schema_value">Value</a>

Object to store information about a single value, which can be a scalar, vector, or matrix.

field name | nullable | value type | description
-----------|----------|------------|------------
name | false | string | Name of the value.
scalar | true | array of [scalar](#schema_scalar) objects | One or more scalars.
vector | true | array of arrays of [scalar](#schema_scalar) objects | One or more vectors. Each sub-array represents a single vector.
matrix | true | array of arrays of arrays of [scalar](#schema_scalar) objects | One or more matrices. Each sub-array-of-arrays represents a single matrix arranged by rows.
units | true | string | Units of the value.

\* While *scalar*, *vector*, and *matrix* are all nullable, exactly one must be non-null.

## <a name="schema_scalar">Scalar</a>

Object to store information about a single scalar value. This field can represent a simple scalar value ("3.4"), a scalar with an uncertainty ("3.4" +- "0.1"), a minimum (>= "3.4"), a maximum (<= "3.4"), or a range ("2.7" - "4.1") using combinations of the available fields.

field name | nullable | value type | description
-----------|----------|------------|------------
value | true* | string | Exact value of the scalar.
minimum | true* | string | Minimum value if this object represents a range.
maximum | true* | string | Maximum value if this object represents a range.
uncertainty | true | string | Uncertainty of *value*, *minimum*, and/or *maximum*.

\* While all fields are nullable, at least one of *value*, *minimum*, and/or *maximum* must be non-null.

## <a name="schema_phase">Phase</a>

Object to store detailed information about a phase in a phase diagram.

field name | nullable | value type | description
-----------|----------|------------|------------
sample | false | single [sample](#schema_sample) object | Details of the phase in a phase diagram.
coordinate | false | array of floating point numbers | Coordinates where the phase appears in the phase diagram.

## <name="schema_point">Point</a>

Object to store a point in a plot or a label in a phase diagram.

field name | nullable | value type | description
-----------|----------|------------|------------
coordinate | false | array of floating point numbers | Coordinates where the point appears.
label | true | string | Text to print at the coordinates of the point.

## <name="schema_line">Line</a>

Object to store a line on a plot or a boundary in a phase diagram.

field name | nullable | value type | description
-----------|----------|------------|------------
coordinate | false | array of arrays of floating point numbers | Coordinates of the points that define a line. Each sub-array represents the coordinates of one point on the line. The line will connect points in the order that they appear.
label | true | string | Text to print on the line.

## <a name="schema_reference">Reference</a>

Object to store information about a referenced work.

field name | nullable | value type | description
-----------|----------|------------|------------
doi | true | string | [Digital Object Identifier](http://www.doi.org) of the reference.
isbn | true | string | [International Standard Book Number](http://www.isbn.org) of the reference.
issn | true | string | [International Standard Serial Number](http://www.issn.org) of the reference.
url | true | string | Internet address of the reference.
title | true | string | Title of the work.
publisher | true | string | Publisher of the work.
journal | true | string | Journal in which the work appeared.
volume | true | string | Volume of the series in which the work appeared.
year | true | string | Year in which the reference was published.
issue | true | string | Issue of the collection in which the work appeared.
pages | true | single [pages](#schema_pages) object | Start and end pages of the work.
author | true | array of [name](#schema_name) objects | List of authors of the work.
editor | true | array of [name](#schema_name) objects | List of editors of the work.
reference | true | array of [reference](#schema_reference) objects | References cited by the work. Reference objects can nest as deeply as needed. This is useful, for example, when tracking the history of a value referenced in a scholarly article; the top level reference would contain information about where the data was accessed while the nested reference would contain information about where it was originally published.

## <a name="schema_person">Person</a>

Object to store information about a person and their contact information.

field name | nullable | value type | description
-----------|----------|------------|------------
name | true* | single [name](#schema_name) object | Name of the person.
email | true* | string | Email address of the person.
orcid | true* | string | [Open Researcher and Contributor ID](http://orcid.org) of the person.

\* While all fields are nullable, at least one must be non-null.

## <a name="schema_name">Name</a>

Object to store the given and family name of a person.

field name | nullable | value type | description
-----------|----------|------------|------------
given | true | string | Given (first) name.
family | false | string | Family (last) name.

## <a name="schema_pages">Pages</a>

Object to store the start and end pages of a reference.

field name | nullable | value type | description
-----------|----------|------------|------------
start | false | string | Starting page of a range.
end | true | string | Ending page of a range.

# <a name="extending">Extending the Schema</a>

All effort should be made to adhere to the [core schema](#core). However, in some cases, it may not be possible to structure some data in this way. Therefore, additional elements may be added by the user where needed as long as they are valid JSON.

Additional objects will be added to the [core schema](#core) over time and any commonly used extensions of the schema will be considered for promotion.

# <a name="file_structure">File Structure</a>

```json
{
    "sample": {
        "material": {},
        "measurement": {}
    }
}
```

A single [MIF object](#core) or an array of [MIF objects](#core) can be stored in a JSON file. Each top level object should contain a single field; the name of that field must be the type of the object that it contains (in [camel case](#camel_case)) and the value of that field set to the actual [MIF object](#core). For example, a file that contains a single [sample](#schema_sample) would appear as:

<div></div>

```json
[
    {
        "sample": {
            "material": {},
            "measurement": {}
        }
    },
    {
        "sample": {
            "material": {},
            "measurement": {}
        }
    }
]
```

Or similarly, a file that contains two [sample](#schema_sample) objects would be written as:

# <a name="examples">Examples</a>

## Superconducting Critical Temperature of RbOs2O6

```json
{
    "sample": {
        "material": {
            "chemicalFormula": "RbOs2O6"
        },
        "measurement": [
            {
                "property": {
                    "units": "K",
                    "scalar": [
                        {
                            "value": "6.3"
                        }
                    ],
                    "name": "Superconducting critical temperature (Tc)"
                },
                "reference": [
                    {
                        "url": "http://arxiv.org/abs/1109.5422v1"
                    },
                    {
                        "doi": "10.1143/jpsj.80.104708"
                    }
                ]
            }
        ]
    }
}
```

This record simply stores the superconducting critical temperature of RbOs2O6 and gives two references: the journal publication for that work as well as the corresponding link to arXiv.

## Band Gap of LiF

```json
{
    "sample": {
        "material": {
            "chemicalFormula": "LiF",
            "condition": [
                {
                    "scalar": [
                        {
                            "value": "Single crystalline"
                        }
                    ],
                    "name": "Crystallinity"
                }
            ]
        },
        "measurement": [
            {
                "dataType": "Experimental",
                "reference": [
                    {
                        "doi": "10.1063/1.3253115"
                    }
                ],
                "property": {
                    "units": "eV",
                    "scalar": [
                        {
                            "value": "13.6"
                        }
                    ],
                    "name": "Band gap"
                },
                "method": "Reflection",
                "condition": [
                    {
                        "scalar": [
                            {
                                "value": "Direct"
                            }
                        ],
                        "name": "Transition"
                    },
                    {
                        "units": "K",
                        "scalar": [
                            {
                                "value": "300"
                            }
                        ],
                        "name": "Temperature"
                    }
                ]
            }
        ]
    }
}
```

This record stores the band gap of single crystalline LiF as 13.6 eV. Additionally, it saves the DOI of the reference from which this value was extracted, that the measurement method was reflection, and that the gap is a direct transition measured at 300 K.

## Properties of CaMnO3 Relevant to Thermoelectric Performance

```json
{
    "sample": {
        "material": {
            "chemicalFormula": "CaMnO3",
            "condition": [
                {
                    "scalar": [
                        {
                            "value": "Polycrystalline"
                        }
                    ],
                    "name": "Crystallinity"
                },
                {
                    "scalar": [
                        {
                            "value": "Solid state reaction"
                        }
                    ],
                    "name": "Preparation method"
                },
                {
                    "scalar": [
                        {
                            "value": "62"
                        }
                    ],
                    "name": "Space group"
                }
            ]
        },
        "measurement": [
            {
                "dataType": "Experimental",
                "property": {
                    "units": "ohm-cm",
                    "scalar": [
                        {
                            "value": "50"
                        }
                    ],
                    "name": "Electrical resistivity"
                },
                "condition": [
                    {
                        "units": "K",
                        "scalar": [
                            {
                                "value": "300"
                            }
                        ],
                        "name": "Temperature"
                    }
                ]
            },
            {
                "dataType": "Experimental",
                "property": {
                    "units": "uV/K",
                    "scalar": [
                        {
                            "value": "-462.97"
                        }
                    ],
                    "name": "Seebeck coefficient"
                },
                "condition": [
                    {
                        "units": "K",
                        "scalar": [
                            {
                                "value": "300"
                            }
                        ],
                        "name": "Temperature"
                    }
                ]
            },
            {
                "dataType": "Experimental",
                "property": {
                    "units": "W/m-K^2",
                    "scalar": [
                        {
                            "value": "4.2868E-07"
                        }
                    ],
                    "name": "Power factor"
                },
                "condition": [
                    {
                        "units": "K",
                        "scalar": [
                            {
                                "value": "300"
                            }
                        ],
                        "name": "Temperature"
                    }
                ]
            },
            {
                "dataType": "Experimental",
                "property": {
                    "units": "S/cm",
                    "scalar": [
                        {
                            "value": "2.0000E-02"
                        }
                    ],
                    "name": "Electrical conductivity"
                },
                "condition": [
                    {
                        "units": "K",
                        "scalar": [
                            {
                                "value": "300"
                            }
                        ],
                        "name": "Temperature"
                    }
                ]
            }
        ],
        "reference": [
            {
                "doi": "10.1021/cm400893e",
                "reference": [
                    {
                        "url": "http://www.jmst.org/EN/Y2009/V25/I04/0535"
                    }
                ]
            }
        ]
    }
}
```

This record stores several properties of polycrystalline CaMnO3 in space group 62, which has been prepared with a solid state reaction. It saves the electrical resistivity at 300 K as 50 ohm-cm, the Seebeck coefficient at 300 K as -462.97 uV/K, the power factor at 300 K as 4.2868E-07 W/m-K^2, and the electrical conductivity at 300 K as 2.0000E-02 S/cm. All values were extracted from the paper with doi 10.1021/cm400893e, which in turn referenced those values from the work at url http://www.jmst.org/EN/Y2009/V25/I04/0535.
