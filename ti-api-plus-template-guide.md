# Topology & Inventory API + Template Guide (Combined)

This file combines the full OpenAPI spec (`topology-inventory-api.yaml`) and the rApp Template Guide (`TEMPLATE_GUIDE.md`) into a single, readable document.

---

## Part 1 — OpenAPI Spec (`topology-inventory-api.yaml`)

```yaml
openapi: 3.0.2
info:
  description: |

    Topology & Inventory data is the information that represents entities
    in a telecommunications network and the relationships between them that
    provide insight into a particular aspect of the network of importance to
    specific use cases. Topology & Inventory data can be derived from
    inventory, configuration, or other data.

    Topology & Inventory supports several topology domains. A domain is a
    grouping of topology & inventory entities that handles topology and
    inventory data.

    Entities are enabling the modelling and storage of complex network
    infrastructure and relationships.

    A relationship is a bi-directional connection between two entities, one
    of which is the originating side (A-side) and the other is the
    terminating side (B-side). The order of the sides matters since it
    defines the relationship itself which must be unique.

    Classifier (also known as tag or label) permits the association of a
    well defined user specified string with an entity or relationship.
    Classifiers can be read using the filtering options and the domains
    endpoint.

    Decorators are user-defined attributes (key-value pairs) which can
    be applied to topology entities and relationships. Decorators can be
    read using the filtering options and the domains endpoint.

    Metadata provides additional information about entities and relationships.
    The reliabilityIndicator is used to indicate the reliability status of the
    topology data within the network. The firstDiscovered timestamp is set for
    an entity and relationship instance when the instances are created for the
    first time in Topology & Inventory. The lastModified timestamp is set for
    updates to entities or relationships in Topology & Inventory, excluding
    updates to classifiers or decorators. The reliabilityIndicator,
    firstDiscovered, and lastModified are implemented as name-value pairs
    within the metadata. They apply to every entity and relationship.

    Topology groups provide the capability to create user-defined collections of
    topology entities and/or relationships of any type. Groups can be either
    static or dynamic based on how they are created.

    Topology & Inventory API provides the capabilities to fetch topology
    data. Using the filtering options, it is possible to define more specific
    query requests.

  version: 1.0.1-alpha.11+7
  title: Topology & Inventory API
servers:
  - url: https://<eic-host>/topology-inventory/v1alpha11
tags:
  - name: Entities and relationships
    description: Provides the capability to retrieve topology & inventory entities, and relationships.
  - name: Schemas
    description: Schemas are defined using the YANG modeling language. A group of YANG schemas make up the Topology & Inventory Data Model, which represents Topology & Inventory entities, their attributes, and their relationships. For more information on the YANG modelling language, see [IETF Documentation](https://datatracker.ietf.org/doc/html/rfc6020). **NOTE:** The POST and DELETE endpoints are applicable only to **user-defined schemas** used for defining classifiers and decorators.
  - name: Classifiers
    description: Provides the capability to update or remove user-defined keywords or tags on entities and relationships.
  - name: Decorators
    description: Provides the capability to update or remove user-defined values on entities and relationships.
  - name: Groups
    description: Provides the capability to group topology entities and/or relationships of any type.
paths:
  /domains:
    get:
      description: Get all the available topology domains.
      tags:
        - Entities and relationships
      summary: Get all the available topology domains.
      operationId: getAllDomains
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Domains'
              examples:
                domains:
                  $ref: '#/components/examples/DomainsResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /domains/{domainName}/entity-types:
    get:
      description: Get all the available topology entity types in domain name.
      tags:
        - Entities and relationships
      summary: Get all the available topology entity types in domain name.
      operationId: getTopologyEntityTypes
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/domainNameInPath'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/EntityTypes'
              examples:
                entityTypes:
                  $ref: '#/components/examples/EntityTypesResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /domains/{domainName}/entity-types/{entityTypeName}/entities:
    get:
      description: Get all topology entities of a specific entity type.
      tags:
        - Entities and relationships
      summary: Get all topology entities of a specific entity type.
      operationId: getTopologyByEntityTypeName
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/domainNameInPath'
        - $ref: '#/components/parameters/entityTypeNameInPath'
        - $ref: '#/components/parameters/targetFilterOptionalInQuery'
        - $ref: '#/components/parameters/scopeFilterOptionalInQuery'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/EntitiesResponseMessage'
              examples:
                entities:
                  $ref: '#/components/examples/EntitiesResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /domains/{domainName}/entity-types/{entityTypeName}/entities/{entityId}:
    get:
      description: Get topology for entity type name with specified id. Specified id represents the entity instance.
      tags:
        - Entities and relationships
      summary: Get topology for entity type name with specified id. Specified id represents the entity instance.
      operationId: getTopologyById
      parameters:
        - $ref: '#/components/parameters/acceptYangJsonInHeader'
        - $ref: '#/components/parameters/domainNameInPath'
        - $ref: '#/components/parameters/entityTypeNameInPath'
        - $ref: '#/components/parameters/entityIdInPath'
      responses:
        '200':
          description: OK
          content:
            application/yang.data+json:
              schema:
                type: object
                description: Refer to yang model for schema definition.
              examples:
                entity:
                  $ref: '#/components/examples/EntityResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /domains/{domainName}/entity-types/{entityTypeName}/entities/{entityId}/relationships:
    get:
      description: Get all relationships for entity type name with specified id. Specified id represents the entity instance.
      tags:
        - Entities and relationships
      summary: Get all relationships for entity type name with specified id. Specified id represents the entity instance.
      operationId: getAllRelationshipsForEntityId
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/domainNameInPath'
        - $ref: '#/components/parameters/entityTypeNameInPath'
        - $ref: '#/components/parameters/entityIdInPath'
        - $ref: '#/components/parameters/targetFilterOptionalInQuery'
        - $ref: '#/components/parameters/scopeFilterOptionalInQuery'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RelationshipsResponseMessage'
              examples:
                relationships:
                  $ref: '#/components/examples/RelationshipsResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /domains/{domainName}/relationship-types:
    get:
      description: Get all the available topology relationship types in a specified domain.
      tags:
        - Entities and relationships
      summary: Get all the available topology relationship types.
      operationId: getTopologyRelationshipTypes
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/domainNameInPath'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RelationshipTypes'
              examples:
                relationshipTypes:
                  $ref: '#/components/examples/RelationshipTypesResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /domains/{domainName}/relationship-types/{relationshipTypeName}/relationships:
    get:
      description: Get topology relationships of a specific relationship type name.
      tags:
        - Entities and relationships
      summary: Get topology relationships of a specific relationship type name.
      operationId: getRelationshipsByType
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/domainNameInPath'
        - $ref: '#/components/parameters/relationshipTypeNameInPath'
        - $ref: '#/components/parameters/targetFilterOptionalInQuery'
        - $ref: '#/components/parameters/scopeFilterOptionalInQuery'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RelationshipsResponseMessage'
              examples:
                relationships:
                  $ref: '#/components/examples/RelationshipsResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /domains/{domainName}/relationship-types/{relationshipTypeName}/relationships/{relationshipId}:
    get:
      description: Get relationship with specified id. Specified id represents the relationship instance.
      tags:
        - Entities and relationships
      summary: Get relationship with specified id. Specified id represents the relationship instance.
      operationId: getRelationshipById
      parameters:
        - $ref: '#/components/parameters/acceptYangJsonInHeader'
        - $ref: '#/components/parameters/domainNameInPath'
        - $ref: '#/components/parameters/relationshipTypeNameInPath'
        - $ref: '#/components/parameters/relationshipIdInPath'
      responses:
        '200':
          description: OK
          content:
            application/yang.data+json:
              schema:
                type: object
                description: Refer to yang model for schema definition.
              examples:
                relationship:
                  $ref: '#/components/examples/RelationshipResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /domains/{domainName}/entities:
    get:
      description: Get topology entities by domain, using a specified *targetFilter* as a query parameter.
      tags:
        - Entities and relationships
      summary: Get entities by domain
      operationId: getEntitiesByDomain
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/domainNameInPath'
        - $ref: '#/components/parameters/targetFilterOptionalInQuery'
        - $ref: '#/components/parameters/scopeFilterOptionalInQuery'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/EntitiesResponseMessage'
              examples:
                entities:
                  $ref: '#/components/examples/EntitiesResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /schemas:
    post:
      description: Create a user-defined schema. The request body contains the schema in YANG format.
      tags:
        - Schemas
      summary: Create a user-defined schema.
      operationId: createSchema
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/contentTypeMultipartFileInHeader'
      requestBody:
        required: true
        content:
          multipart/form-data:
            schema:
              $ref: '#/components/schemas/MultipartFile'
      responses:
        '201':
          $ref: '#/components/responses/Created'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '409':
          $ref: '#/components/responses/Conflict'
        '500':
          $ref: '#/components/responses/InternalServerError'
    get:
      description: Get a list of all schemas.
      tags:
        - Schemas
      summary: Get a list of all schemas.
      operationId: getSchemas
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/domainOptionalInQuery'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SchemaList'
              examples:
                schemas:
                  $ref: '#/components/examples/SchemasResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /schemas/{schemaName}/content:
    get:
      description: Get the model schema by name.
      tags:
        - Schemas
      summary: Get the model schema.
      operationId: getSchemaByName
      parameters:
        - $ref: '#/components/parameters/acceptPlainTextInHeader'
        - $ref: '#/components/parameters/schemaNameInPath'
      responses:
        '200':
          description: OK
          content:
            text/plain:
              schema:
                type: string
              examples:
                schema:
                  $ref: '#/components/examples/SchemaResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /schemas/{schemaName}:
    delete:
      description: Delete a user-defined schema.
      tags:
        - Schemas
      summary: Delete a user-defined schema.
      operationId: deleteSchema
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/schemaNameInDeletePath'
      responses:
        '204':
          $ref: '#/components/responses/NoContent'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /classifiers:
    post:
      description: Update entities and/or relationships with classifier(s). The sum of the given entityIds and relationshipIds cannot exceed 100 by default.
      tags:
        - Classifiers
      summary: Update entities and/or relationships with classifier(s).
      operationId: updateClassifier
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/contentTypeJsonInHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Classifier'
            examples:
              updateClassifier:
                $ref: '#/components/examples/ClassifierMergeExample'
              removeClassifier:
                $ref: '#/components/examples/ClassifierDeleteExample'
      responses:
        '204':
          $ref: '#/components/responses/NoContent'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '409':
          $ref: '#/components/responses/Conflict'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /decorators:
    post:
      description: Update entities and/or relationships with decorator(s). The sum of the given entityIds and relationshipIds cannot exceed 100 by default.
      tags:
        - Decorators
      summary: Update entities and/or relationships with decorator(s).
      operationId: updateDecorator
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/contentTypeJsonInHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/Decorator'
            examples:
              mergeDecorator:
                $ref: '#/components/examples/DecoratorMergeExample'
              removeDecorator:
                $ref: '#/components/examples/DecoratorDeleteExample'
      responses:
        '204':
          $ref: '#/components/responses/NoContent'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '409':
          $ref: '#/components/responses/Conflict'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /groups:
    post:
      description: Create a group of entities and/or relationships in a static or dynamic way.
      tags:
        - Groups
      summary: Create a new group.
      operationId: createGroup
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/contentTypeJsonInHeader'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateGroupPayload'
            examples:
              staticGroup:
                $ref: '#/components/examples/CreateStaticGroupPayloadExample'
              dynamicGroup:
                $ref: '#/components/examples/CreateDynamicGroupGetEntitiesByDomainPayloadExample'
      responses:
        '201':
          description: Created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/GroupByIdResponse'
              examples:
                static:
                  $ref: '#/components/examples/StaticGroupResponseExample'
                dynamic:
                  $ref: '#/components/examples/DynamicGroupResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '500':
          $ref: '#/components/responses/InternalServerError'
    get:
      description: Get all groups.
      tags:
        - Groups
      summary: Get all groups.
      operationId: getAllGroups
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
        - $ref: '#/components/parameters/groupNameOptionalInQuery'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/GroupsResponse'
              examples:
                groups:
                  $ref: '#/components/examples/GroupsResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /groups/{groupId}:
    get:
      description: Get a group with specified id.
      tags:
        - Groups
      summary: Get a group with specified id.
      operationId: getGroupById
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/groupIdInPath'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/GroupByIdResponse'
              examples:
                static:
                  $ref: '#/components/examples/StaticGroupResponseExample'
                dynamic:
                  $ref: '#/components/examples/DynamicGroupResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
    delete:
      description: Delete a group with specified id.
      tags:
        - Groups
      summary: Delete a group with specified id.
      operationId: deleteGroup
      parameters:
        - $ref: '#/components/parameters/groupIdInPath'
      responses:
        '204':
          $ref: '#/components/responses/NoContent'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /groups/{groupId}/name:
    put:
      description: Update the name of a group.
      tags:
        - Groups
      summary: Update the name of a group.
      operationId: updateGroupName
      parameters:
        - $ref: '#/components/parameters/contentTypeJsonInHeader'
        - $ref: '#/components/parameters/groupIdInPath'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateGroupNamePayload'
            examples:
              GroupNameUpdatePayload:
                $ref: '#/components/examples/UpdateGroupNamePayloadExample'
      responses:
        '204':
          $ref: '#/components/responses/NoContent'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /groups/{groupId}/members:
    get:
      description: Get the members of a group with specified id.
      tags:
        - Groups
      summary: Get the members of a group with specified id.
      operationId: getMembers
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/groupIdInPath'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MembersResponse'
              examples:
                members:
                  $ref: '#/components/examples/MembersResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /groups/{groupId}/provided-members:
    get:
      description: Get the provided members of a static group with specified id.
      tags:
        - Groups
      summary: Get the provided members of a static group with specified id.
      operationId: getProvidedMembers
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/groupIdInPath'
        - $ref: '#/components/parameters/groupMembersStatusOptionalInQuery'
        - $ref: '#/components/parameters/offsetParam'
        - $ref: '#/components/parameters/limitParam'
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MembersResponse'
              examples:
                members:
                  $ref: '#/components/examples/ProvidedMembersResponseExample'
        '400':
          $ref: '#/components/responses/BadRequest'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/InternalServerError'
  /groups/{groupId}/provided-members-operations:
    post:
      description: Merge or remove members in an existing topology group. This operation is applicable for static group only.
      tags:
        - Groups
      summary: Merge or remove members of a static group.
      operationId: updateProvidedMembers
      parameters:
        - $ref: '#/components/parameters/acceptJsonInHeader'
        - $ref: '#/components/parameters/contentTypeJsonInHeader'
        - $ref: '#/components/parameters/groupIdInPath'
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/UpdateProvidedMembersPayload'
            examples:
              mergeMembersPayload:
                $ref: '#/components/examples/MergeProvidedMembersPayloadExample'
              deleteMembersPayload:
                $ref: '#/components/examples/RemoveProvidedMembersPayloadExample'
      responses:
        '204':
          $ref: '#/components/responses/NoContent'
        '400':
          $ref: '#/components/responses/BadRequest'
        '500':
          $ref: '#/components/responses/InternalServerError'
components:
  schemas:
    Classifier:
      type: object
      title: Classifier
      properties:
        operation:
          type: string
          enum:
            - merge
            - delete
        classifiers:
          type: array
          items:
            type: string
        entityIds:
          type: array
          items:
            type: string
        relationshipIds:
          type: array
          items:
            type: string
    Decorator:
      type: object
      title: Decorator
      properties:
        operation:
          type: string
          enum:
            - merge
            - delete
        decorators:
          type: object
          additionalProperties: true
          description: Decorators must be defined in schema before use. Data type of a decorator is restricted as defined by its schema.
        entityIds:
          type: array
          items:
            type: string
        relationshipIds:
          type: array
          items:
            type: string
    Domains:
      type: object
      title: Domains
      properties:
        items:
          type: array
          items:
            properties:
              name:
                type: string
              entityTypes:
                $ref: '#/components/schemas/Href'
              relationshipTypes:
                $ref: '#/components/schemas/Href'
        self:
          $ref: '#/components/schemas/Href'
        first:
          $ref: '#/components/schemas/Href'
        prev:
          $ref: '#/components/schemas/Href'
        next:
          $ref: '#/components/schemas/Href'
        last:
          $ref: '#/components/schemas/Href'
        totalCount:
          type: integer
    EntityTypes:
      type: object
      title: EntityTypes
      properties:
        items:
          type: array
          items:
            type: object
            properties:
              name:
                type: string
              entities:
                $ref: '#/components/schemas/Href'
        self:
          $ref: '#/components/schemas/Href'
        first:
          $ref: '#/components/schemas/Href'
        prev:
          $ref: '#/components/schemas/Href'
        next:
          $ref: '#/components/schemas/Href'
        last:
          $ref: '#/components/schemas/Href'
        totalCount:
          type: integer
    EntitiesResponseMessage:
      type: object
      title: Entities
      properties:
        items:
          type: array
          items:
            type: object
            description: Refer to yang model for schema definition of topology entities.
        self:
          $ref: '#/components/schemas/Href'
        first:
          $ref: '#/components/schemas/Href'
        prev:
          $ref: '#/components/schemas/Href'
        next:
          $ref: '#/components/schemas/Href'
        last:
          $ref: '#/components/schemas/Href'
        totalCount:
          type: integer
    RelationshipTypes:
      type: object
      title: RelationshipTypes
      properties:
        items:
          type: array
          items:
            type: object
            properties:
              name:
                type: string
              relationships:
                $ref: '#/components/schemas/Href'
        self:
          $ref: '#/components/schemas/Href'
        first:
          $ref: '#/components/schemas/Href'
        prev:
          $ref: '#/components/schemas/Href'
        next:
          $ref: '#/components/schemas/Href'
        last:
          $ref: '#/components/schemas/Href'
        totalCount:
          type: integer
    RelationshipsResponseMessage:
      type: object
      title: Relationships
      properties:
        items:
          type: array
          items:
            type: object
            description: Refer to yang model for schema definition of topology relationships.
        self:
          $ref: '#/components/schemas/Href'
        first:
          $ref: '#/components/schemas/Href'
        prev:
          $ref: '#/components/schemas/Href'
        next:
          $ref: '#/components/schemas/Href'
        last:
          $ref: '#/components/schemas/Href'
        totalCount:
          type: integer
    ErrorMessage:
      type: object
      title: Error
      properties:
        status:
          type: string
        message:
          type: string
        details:
          type: string
    Href:
      type: object
      title: Href
      properties:
        href:
          type: string
          format: uri-template
    MultipartFile:
      type: object
      required:
        - file
      properties:
        file:
          type: string
          description: multipartFile
          format: binary
    Schema:
      type: object
      title: Schema
      properties:
        name:
          type: string
        domain:
          type: string
        revision:
          type: string
        content:
          $ref: '#/components/schemas/Href'
    SchemaList:
      type: object
      title: Schemas
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/Schema'
        self:
          $ref: '#/components/schemas/Href'
        first:
          $ref: '#/components/schemas/Href'
        prev:
          $ref: '#/components/schemas/Href'
        next:
          $ref: '#/components/schemas/Href'
        last:
          $ref: '#/components/schemas/Href'
        totalCount:
          type: integer
    StaticEnum:
      type: string
      enum:
        - static
    DynamicEnum:
      type: string
      enum:
        - dynamic
    CreateGroupPayload:
      title: CreateGroupPayload
      type: object
      oneOf:
        - $ref: '#/components/schemas/static'
        - $ref: '#/components/schemas/dynamic'
      discriminator:
        propertyName: type
    static:
      title: CreateStaticGroupPayload
      type: object
      required:
        - name
        - type
        - providedMembers
      properties:
        name:
          type: string
          description: A name of the topology group.
          minLength: 1
          maxLength: 150
        type:
          type: string
          description: 'Allowed: static'
        providedMembers:
          type: array
          minItems: 1
          items:
            type: object
            description: Refer to yang model for schema definition of topology objects.
    dynamic:
      title: CreateDynamicGroupPayload
      type: object
      required:
        - name
        - type
        - criteria
      properties:
        name:
          type: string
          description: A name of the topology group.
          minLength: 1
          maxLength: 150
        type:
          type: string
          description: 'Allowed: dynamic'
        criteria:
          $ref: '#/components/schemas/Criteria'
    Criteria:
      title: Criteria
      type: object
      oneOf:
        - $ref: '#/components/schemas/getEntitiesByDomain'
        - $ref: '#/components/schemas/getEntitiesByType'
        - $ref: '#/components/schemas/getRelationshipsForEntityId'
        - $ref: '#/components/schemas/getRelationshipsByType'
      discriminator:
        propertyName: queryType
    getEntitiesByDomain:
      title: getEntitiesByDomain
      type: object
      required:
        - queryType
        - domain
      properties:
        queryType:
          type: string
          description: 'Allowed: getEntitiesByDomain'
        domain:
          type: string
        targetFilter:
          type: string
        scopeFilter:
          type: string
    getEntitiesByType:
      title: getEntitiesByType
      type: object
      required:
        - queryType
        - domain
        - entityTypeName
      properties:
        queryType:
          type: string
          description: 'Allowed: getEntitiesByType'
        domain:
          type: string
        entityTypeName:
          type: string
        targetFilter:
          type: string
        scopeFilter:
          type: string
    getRelationshipsForEntityId:
      title: getRelationshipsForEntityId
      type: object
      required:
        - queryType
        - domain
        - entityTypeName
        - entityId
      properties:
        queryType:
          type: string
          description: 'Allowed: getRelationshipsForEntityId'
        domain:
          type: string
        entityTypeName:
          type: string
        entityId:
          type: string
        targetFilter:
          type: string
        scopeFilter:
          type: string
    getRelationshipsByType:
      title: getRelationshipsByType
      type: object
      required:
        - queryType
        - domain
        - relationshipTypeName
      properties:
        queryType:
          type: string
          description: 'Allowed: getRelationshipsByType'
        domain:
          type: string
        relationshipTypeName:
          type: string
        targetFilter:
          type: string
        scopeFilter:
          type: string
    GroupsResponse:
      title: Groups
      type: object
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/GroupResponse'
        self:
          $ref: '#/components/schemas/Href'
        first:
          $ref: '#/components/schemas/Href'
        prev:
          $ref: '#/components/schemas/Href'
        next:
          $ref: '#/components/schemas/Href'
        last:
          $ref: '#/components/schemas/Href'
        totalCount:
          type: integer
    GroupResponse:
      title: Group
      type: object
      oneOf:
        - $ref: '#/components/schemas/StaticGroupResponse'
        - $ref: '#/components/schemas/DynamicGroupResponse'
    StaticGroupResponse:
      title: StaticGroup
      type: object
      required:
        - id
        - name
        - type
        - members
        - providedMembers
      properties:
        id:
          type: string
          description: The unique identifier of the topology group.
        name:
          type: string
          description: The unique name of the topology group.
        type:
          $ref: '#/components/schemas/StaticEnum'
        members:
          $ref: '#/components/schemas/Href'
        providedMembers:
          $ref: '#/components/schemas/Href'
    DynamicGroupResponse:
      title: DynamicGroup
      type: object
      required:
        - id
        - name
        - type
        - members
      properties:
        id:
          type: string
          description: The unique identifier of the topology group.
        name:
          type: string
          description: The unique name of the topology group.
        type:
          $ref: '#/components/schemas/DynamicEnum'
        members:
          $ref: '#/components/schemas/Href'
    GroupByIdResponse:
      title: Group
      type: object
      oneOf:
        - $ref: '#/components/schemas/StaticGroupByIdResponse'
        - $ref: '#/components/schemas/DynamicGroupByIdResponse'
    StaticGroupByIdResponse:
      title: StaticGroup
      type: object
      required:
        - id
        - name
        - type
        - members
        - providedMembers
      properties:
        id:
          type: string
          description: The unique identifier of the topology group.
        name:
          type: string
          description: The unique name of the topology group.
        type:
          $ref: '#/components/schemas/StaticEnum'
        members:
          $ref: '#/components/schemas/Href'
        providedMembers:
          $ref: '#/components/schemas/Href'
    DynamicGroupByIdResponse:
      title: DynamicGroup
      type: object
      required:
        - id
        - name
        - type
        - members
        - criteria
      properties:
        id:
          type: string
          description: The unique identifier of the topology group.
        name:
          type: string
          description: The unique name of the topology group.
        type:
          $ref: '#/components/schemas/DynamicEnum'
        members:
          $ref: '#/components/schemas/Href'
        criteria:
          $ref: '#/components/schemas/Criteria'
    MembersResponse:
      title: GroupMembers
      type: object
      properties:
        items:
          type: array
          items:
            type: object
            description: Refer to yang model for schema definition of topology objects.
        self:
          $ref: '#/components/schemas/Href'
        first:
          $ref: '#/components/schemas/Href'
        prev:
          $ref: '#/components/schemas/Href'
        next:
          $ref: '#/components/schemas/Href'
        last:
          $ref: '#/components/schemas/Href'
        totalCount:
          type: integer
    UpdateProvidedMembersPayload:
      title: UpdateProvidedMembersPayload
      type: object
      required:
        - operation
        - providedMembers
      properties:
        operation:
          type: string
          description: The operation to be performed on the members of topology group.
          enum:
            - merge
            - remove
        providedMembers:
          description: Members to be added or removed from the group.
          type: array
          minItems: 1
          items:
            type: object
            description: Refer to yang model for schema definition of topology objects.
    UpdateGroupNamePayload:
      title: UpdateGroupNamePayload
      type: object
      required:
        - name
      properties:
        name:
          type: string
          minLength: 1
          maxLength: 150
  responses:
    NotFound:
      description: Not Found
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ErrorMessage'
          example:
            status: '404'
            message: Resource Not Found
            details: The requested resource is not found
    Unauthorized:
      description: Unauthorized
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ErrorMessage'
          example:
            status: '401'
            message: Unauthorized request
            details: This request is unauthorized
    Forbidden:
      description: Forbidden
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ErrorMessage'
          example:
            status: '403'
            message: Request Forbidden
            details: This request is forbidden
    BadRequest:
      description: Bad Request
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ErrorMessage'
          example:
            status: '400'
            message: Bad Request
            details: The provided request is not valid
    Conflict:
      description: Conflict
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ErrorMessage'
          example:
            status: '409'
            message: Conflicting request
            details: The request cannot be processed as the resource is in use.
    Created:
      description: Created without response body
    InternalServerError:
      description: Internal Server Error
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/ErrorMessage'
          example:
            status: '500'
            message: Internal Server Error
            details: Internal Server Error occurred
    NoContent:
      description: No Content
      content: {}
  parameters:
    acceptJsonInHeader:
      name: Accept
      in: header
      required: true
      schema:
        type: string
        example: application/json
        default: application/json
    acceptPlainTextInHeader:
      name: Accept
      in: header
      required: true
      schema:
        type: string
        example: text/plain
        default: text/plain
    acceptYangJsonInHeader:
      name: Accept
      in: header
      required: true
      schema:
        type: string
        example: application/yang.data+json
        default: application/yang.data+json
    contentTypeMultipartFileInHeader:
      name: Content-Type
      in: header
      required: true
      schema:
        type: string
        example: multipart/form-data
        default: multipart/form-data
    contentTypeJsonInHeader:
      name: Content-Type
      in: header
      required: true
      schema:
        type: string
        example: application/json
        default: application/json
    offsetParam:
      name: offset
      in: query
      description: Pagination offset.
      required: false
      schema:
        type: integer
        default: 0
        minimum: 0
    limitParam:
      name: limit
      in: query
      description: Result limiter.
      required: false
      schema:
        type: integer
        default: 500
        minimum: 1
        maximum: 500
    domainNameInPath:
      name: domainName
      in: path
      description: Domain name
      required: true
      schema:
        type: string
        example: RAN
    schemaNameInPath:
      name: schemaName
      in: path
      required: true
      description: "Path parameter to specify the name of the module to fetch."
      schema:
        type: string
        example: o-ran-smo-teiv-oam
    schemaNameInDeletePath:
      name: schemaName
      in: path
      required: true
      description: "Path parameter to specify the name of a user-defined module to delete."
      schema:
        type: string
        example: classifier-rapp-module
    entityIdInPath:
      name: entityId
      in: path
      required: true
      schema:
        type: string
    relationshipIdInPath:
      name: relationshipId
      in: path
      required: true
      schema:
        type: string
    entityTypeNameInPath:
      name: entityTypeName
      in: path
      required: true
      schema:
        type: string
        example: NRCellDU
    relationshipTypeNameInPath:
      name: relationshipTypeName
      in: path
      required: true
      schema:
        type: string
        example: NRCELLDU_USES_NRSECTORCARRIER
    groupIdInPath:
      name: groupId
      in: path
      required: true
      schema:
        type: string
    domainOptionalInQuery:
      name: domain
      in: query
      required: false
      description: "Query parameter to filter the result set to only return modules that match the provided domain. <br>**Example:** OAM"
      schema:
        type: string
    targetFilterOptionalInQuery:
      name: targetFilter
      description: Use *targetFilter* to specify what needs to be returned in the REST response.
      in: query
      required: false
      schema:
        type: string
      examples:
        targetFilter:
          value: /sourceIds;/classifiers
    scopeFilterOptionalInQuery:
      name: scopeFilter
      description: ScopeFilter is used to specify the conditions to be applied.
      in: query
      required: false
      schema:
        type: string
      examples:
        scopeFilter:
          value: /sourceIds[contains(@item,'ManagedElement=1')]
    groupMembersStatusOptionalInQuery:
      name: status
      description: Status can be present (or) not-present (or) invalid. If not specified, returns all members of the group.
      in: query
      required: false
      schema:
        type: string
        enum:
          - present
          - not-present
          - invalid
    groupNameOptionalInQuery:
      name: name
      description: Group name. If not specified, returns all the groups.
      in: query
      required: false
      schema:
        type: string
  examples:
    ClassifierMergeExample:
      value:
        operation: merge
        classifiers:
          - module-x:Outdoor
          - module-y:Rural
          - module-z:Weekend
        entityIds:
          - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
          - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=2
        relationshipIds:
          - urn:o-ran:smo:teiv:sha512:NRCELLDU_USES_NRSECTORCARRIER=CA576F4716C36A1BD1C506DCB58418FC731858D3D3F856F536813A8C4D3F1CC21292E506815410E04496D709D96066EBC0E4890DEFC3789EDC4BD9C28DA1D52B
    ClassifierDeleteExample:
      value:
        operation: delete
        classifiers:
          - module-x:Outdoor
          - module-z:Weekend
        entityIds:
          - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
          - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=2
        relationshipIds:
          - urn:o-ran:smo:teiv:sha512:NRCELLDU_USES_NRSECTORCARRIER=CA576F4716C36A1BD1C506DCB58418FC731858D3D3F856F536813A8C4D3F1CC21292E506815410E04496D709D96066EBC0E4890DEFC3789EDC4BD9C28DA1D52B
    DecoratorMergeExample:
      value:
        operation: merge
        decorators:
          module-x:location: Stockholm
          module-y:vendor: Ericsson
        entityIds:
          - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
          - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=2
        relationshipIds:
          - urn:o-ran:smo:teiv:sha512:NRCELLDU_USES_NRSECTORCARRIER=CA576F4716C36A1BD1C506DCB58418FC731858D3D3F856F536813A8C4D3F1CC21292E506815410E04496D709D96066EBC0E4890DEFC3789EDC4BD9C28DA1D52B
    DecoratorDeleteExample:
      value:
        operation: delete
        decorators:
          module-x:location: Stockholm
        entityIds:
          - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
          - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=2
        relationshipIds:
          - urn:o-ran:smo:teiv:sha512:NRCELLDU_USES_NRSECTORCARRIER=CA576F4716C36A1BD1C506DCB58418FC731858D3D3F856F536813A8C4D3F1CC21292E506815410E04496D709D96066EBC0E4890DEFC3789EDC4BD9C28DA1D52B
    EntityResponseExample:
      value:
        o-ran-smo-teiv-ran:NRCellDU:
          - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
            attributes:
              cellLocalId: 91
              nCI: 91
              nRPCI: 789
              nRTAC: 456
            decorators:
              location: Stockholm
            classifiers:
              - Rural
            sourceIds:
              - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
              - urn:cmHandle:395221E080CCF0FD1924103B15873814
            metadata:
              reliabilityIndicator: OK
              firstDiscovered: '2025-01-07T12:20:12.24523200Z'
              lastModified: '2025-01-08T10:40:36.46156500Z'
    EntitiesResponseExample:
      value:
        items:
          - o-ran-smo-teiv-ran:GNBCUUPFunction:
              - id: urn:3gpp:dn:SubNetwork=Europe,SubNetwork=Hungary,MeContext=1,ManagedElement=10,GNBCUUPFunction=10
                attributes:
                  gNBId: 10
                  gNBIdLength: 2
                sourceIds:
                  - urn:3gpp:dn:SubNetwork=Europe,SubNetwork=Hungary,MeContext=1,ManagedElement=10,GNBCUUPFunction=10
                  - urn:cmHandle:72FDA73D085F138FECC974CB91F1450E
                metadata:
                  reliabilityIndicator: OK
                  firstDiscovered: '2025-01-07T12:20:12.24523200Z'
                  lastModified: '2025-01-08T10:40:36.46156500Z'
          - o-ran-smo-teiv-ran:GNBCUUPFunction:
              - id: urn:3gpp:dn:SubNetwork=Europe,SubNetwork=Hungary,MeContext=1,ManagedElement=13,GNBCUUPFunction=13
                attributes:
                  gNBId: 13
                  gNBIdLength: 2
                sourceIds:
                  - urn:3gpp:dn:SubNetwork=Europe,SubNetwork=Hungary,MeContext=1,ManagedElement=13,GNBCUUPFunction=13
                  - urn:cmHandle:E5196035D0B49A65B00EAA392B4EE155
                metadata:
                  reliabilityIndicator: OK
                  firstDiscovered: '2025-01-07T12:20:12.24523200Z'
                  lastModified: '2025-01-08T10:40:36.46156500Z'
          - o-ran-smo-teiv-ran:GNBCUUPFunction:
              - id: urn:3gpp:dn:SubNetwork=Europe,SubNetwork=Hungary,MeContext=1,ManagedElement=14,GNBCUUPFunction=14
                attributes:
                  gNBId: 14
                  gNBIdLength: 2
                sourceIds:
                  - urn:3gpp:dn:SubNetwork=Europe,SubNetwork=Hungary,MeContext=1,ManagedElement=14,GNBCUUPFunction=14
                  - urn:cmHandle:D67C0BD04FA613BBFD176B24B68FD6A4
                metadata:
                  reliabilityIndicator: OK
                  firstDiscovered: '2025-01-07T12:20:12.24523200Z'
                  lastModified: '2025-01-08T10:40:36.46156500Z'
        self:
          href: /domains/RAN/entities?offset=0&limit=3&targetFilter=/sourceIds;/attributes
        first:
          href: /domains/RAN/entities?offset=0&limit=3&targetFilter=/sourceIds;/attributes
        prev:
          href: /domains/RAN/entities?offset=0&limit=3&targetFilter=/sourceIds;/attributes
        next:
          href: /domains/RAN/entities?offset=3&limit=3&targetFilter=/sourceIds;/attributes
        last:
          href: /domains/RAN/entities?offset=33&limit=3&targetFilter=/sourceIds;/attributes
        totalCount: 3
    RelationshipResponseExample:
      value:
        o-ran-smo-teiv-ran:NRCELLDU_USES_NRSECTORCARRIER:
          - id: urn:o-ran:smo:teiv:sha512:NRCELLDU_USES_NRSECTORCARRIER=CA576F4716C36A1BD1C506DCB58418FC731858D3D3F856F536813A8C4D3F1CC21292E506815410E04496D709D96066EBC0E4890DEFC3789EDC4BD9C28DA1D52B
            aSide: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
            bSide: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRSectorCarrier=1
            sourceIds:
              - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
              - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRSectorCarrier=1
            metadata:
              reliabilityIndicator: OK
              firstDiscovered: '2025-01-07T12:20:12.24523200Z'
              lastModified: '2025-01-08T10:40:36.46156500Z'
    RelationshipsResponseExample:
      value:
        items:
          - o-ran-smo-teiv-ran:NRCELLDU_USES_NRSECTORCARRIER:
              - id: urn:o-ran:smo:teiv:sha512:NRCELLDU_USES_NRSECTORCARRIER=CA576F4716C36A1BD1C506DCB58418FC731858D3D3F856F536813A8C4D3F1CC21292E506815410E04496D709D96066EBC0E4890DEFC3789EDC4BD9C28DA1D52B
                aSide: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
                bSide: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRSectorCarrier=1
                sourceIds:
                  - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
                  - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRSectorCarrier=1
                metadata:
                  reliabilityIndicator: OK
                  firstDiscovered: '2025-01-07T12:20:12.24523200Z'
                  lastModified: '2025-01-08T10:40:36.46156500Z'
          - o-ran-smo-teiv-ran:NRCELLDU_USES_NRSECTORCARRIER:
              - id: urn:o-ran:smo:teiv:sha512:NRCELLDU_USES_NRSECTORCARRIER=11AB21444F9D7C6DAC7453879AB5586D294B495E43AC6F94750767DD624014DB7317E9A5EE73239876649D801037D6347355B19C5D97222B3C25000CF8A97C78
                aSide: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=2
                bSide: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRSectorCarrier=2
                sourceIds:
                  - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=2
                  - urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRSectorCarrier=2
                metadata:
                  reliabilityIndicator: OK
                  firstDiscovered: '2025-01-07T12:20:12.24523200Z'
                  lastModified: '2025-01-08T10:40:36.46156500Z'
        self:
          href: /domains/RAN/relationship-types/NRCELLDU_USES_NRSECTORCARRIER/relationships?offset=0&limit=500
        first:
          href: /domains/RAN/relationship-types/NRCELLDU_USES_NRSECTORCARRIER/relationships?offset=0&limit=500
        prev:
          href: /domains/RAN/relationship-types/NRCELLDU_USES_NRSECTORCARRIER/relationships?offset=0&limit=500
        next:
          href: /domains/RAN/relationship-types/NRCELLDU_USES_NRSECTORCARRIER/relationships?offset=0&limit=500
        last:
          href: /domains/RAN/relationship-types/NRCELLDU_USES_NRSECTORCARRIER/relationships?offset=0&limit=500
        totalCount: 2
    EntityTypesResponseExample:
      value:
        items:
          - name: AntennaCapability
            entities:
              href: /domains/RAN/entity-types/AntennaCapability/entities
          - name: ENodeBFunction
            entities:
              href: /domains/RAN/entity-types/ENodeBFunction/entities
          - name: EUtranCell
            entities:
              href: /domains/RAN/entity-types/EUtranCell/entities
        self:
          href: /domains/RAN/entity-types?offset=0&limit=3
        first:
          href: /domains/RAN/entity-types?offset=0&limit=3
        prev:
          href: /domains/RAN/entity-types?offset=0&limit=3
        next:
          href: /domains/RAN/entity-types?offset=3&limit=3
        last:
          href: /domains/RAN/entity-types?offset=9&limit=3
        totalCount: 11
    RelationshipTypesResponseExample:
      value:
        items:
          - name: MANAGEDELEMENT_MANAGES_GNBDUFUNCTION
            relationships:
              href: /domains/RAN/relationship-types/MANAGEDELEMENT_MANAGES_GNBDUFUNCTION/relationships
          - name: GNBDUFUNCTION_PROVIDES_NRCELLDU
            relationships:
              href: /domains/RAN/relationship-types/GNBDUFUNCTION_PROVIDES_NRCELLDU/relationships
          - name: NRCELLDU_USES_NRSECTORCARRIER
            relationships:
              href: /domains/RAN/relationship-types/NRCELLDU_USES_NRSECTORCARRIER/relationships
        self:
          href: /domains/RAN/relationship-types?offset=0&limit=3
        first:
          href: /domains/RAN/relationship-types?offset=0&limit=3
        prev:
          href: /domains/RAN/relationship-types?offset=0&limit=3
        next:
          href: /domains/RAN/relationship-types?offset=0&limit=3
        last:
          href: /domains/RAN/relationship-types?offset=0&limit=3
        totalCount: 3
    DomainsResponseExample:
      value:
        items:
          - name: EQUIPMENT
            entityTypes:
              href: /domains/EQUIPMENT/entity-types
            relationshipTypes:
              href: /domains/EQUIPMENT/relationship-types
          - name: OAM
            entityTypes:
              href: /domains/OAM/entity-types
            relationshipTypes:
              href: /domains/OAM/relationship-types
          - name: RAN
            entityTypes:
              href: /domains/RAN/entity-types
            relationshipTypes:
              href: /domains/RAN/relationship-types
          - name: REL_EQUIPMENT_RAN
            entityTypes:
              href: /domains/REL_EQUIPMENT_RAN/entity-types
            relationshipTypes:
              href: /domains/REL_EQUIPMENT_RAN/relationship-types
          - name: REL_OAM_RAN
            entityTypes:
              href: /domains/REL_OAM_RAN/entity-types
            relationshipTypes:
              href: /domains/REL_OAM_RAN/relationship-types
          - name: TEIV
            entityTypes:
              href: /domains/TEIV/entity-types
            relationshipTypes:
              href: /domains/TEIV/relationship-types
        self:
          href: /domains?offset=0&limit=500
        first:
          href: /domains?offset=0&limit=500
        prev:
          href: /domains?offset=0&limit=500
        next:
          href: /domains?offset=0&limit=500
        last:
          href: /domains?offset=0&limit=500
        totalCount: 6
    SchemasResponseExample:
      value:
        items:
          - name: o-ran-smo-teiv-ran
            domain: RAN
            revision: '2024-05-24'
            content:
              href: /schemas/o-ran-smo-teiv-ran/content
          - name: o-ran-smo-teiv-equipment
            domain: EQUIPMENT
            revision: '2024-05-24'
            content:
              href: /schemas/o-ran-smo-teiv-equipment/content
          - name: o-ran-smo-teiv-oam
            domain: OAM
            revision: '2024-05-24'
            content:
              href: /schemas/o-ran-smo-teiv-oam/content
          - name: o-ran-smo-teiv-rel-oam-ran
            domain: REL_OAM_RAN
            revision: '2024-05-24'
            content:
              href: /schemas/o-ran-smo-teiv-rel-oam-ran/content
          - name: o-ran-smo-teiv-rel-equipment-ran
            domain: REL_EQUIPMENT_RAN
            revision: '2024-05-24'
            content:
              href: /schemas/o-ran-smo-teiv-rel-equipment-ran/content
          - name: o-ran-smo-teiv-common-yang-types
            domain: ''
            revision: '2024-05-24'
            content:
              href: /schemas/o-ran-smo-teiv-common-yang-types/content
          - name: o-ran-smo-teiv-common-yang-extensions
            domain: ''
            revision: '2024-05-24'
            content:
              href: /schemas/o-ran-smo-teiv-common-yang-extensions/content
          - name: ietf-geo-location
            domain: ''
            revision: '2022-02-11'
            content:
              href: /schemas/ietf-geo-location/content
          - name: _3gpp-common-yang-extensions
            domain: ''
            revision: '2019-06-23'
            content:
              href: /schemas/_3gpp-common-yang-extensions/content
          - name: _3gpp-common-yang-types
            domain: ''
            revision: '2023-11-06'
            content:
              href: /schemas/_3gpp-common-yang-types/content
          - name: ietf-yang-types
            domain: ''
            revision: '2013-07-15'
            content:
              href: /schemas/ietf-yang-types/content
          - name: ietf-inet-types
            domain: ''
            revision: '2013-07-15'
            content:
              href: /schemas/ietf-inet-types/content
        self:
          href: /schemas?offset=0&limit=500
        first:
          href: /schemas?offset=0&limit=500
        prev:
          href: /schemas?offset=0&limit=500
        next:
          href: /schemas?offset=0&limit=500
        last:
          href: /schemas?offset=0&limit=500
        totalCount: 12
    SchemaResponseExample:
      value: |
        module o-ran-smo-teiv-oam {
            yang-version 1.1;
            namespace "urn:o-ran:smo-teiv-oam";
            prefix or-teiv-oam;

            import o-ran-smo-teiv-common-yang-types { prefix or-teiv-types; }

            import o-ran-smo-teiv-common-yang-extensions { prefix or-teiv-yext; }

            organization "Ericsson AB";
            contact "Ericsson first line support via email";
            description
            "O&M topology model.

            Copyright (c) 2024 Ericsson AB. All rights reserved.

            This model contains the topology entities and relations in the O&M domain,
            which are intended to represent management systems and management
            interfaces.";

            revision "2024-10-04" {
                description "Added grouping, Origin_Entity_Mapping_Grp to the topology object.";
                or-teiv-yext:label 0.4.0;
            }

            revision "2024-05-24" {
                description "Initial revision.";
                or-teiv-yext:label 0.3.0;
            }

            or-teiv-yext:domain OAM;

            list ManagedElement {
                description "A Managed Element (ME) is a node into a telecommunication
                    network providing support and/or service to subscribers. An ME
                    communicates with a manager application (directly or indirectly)
                    over one or more interfaces for the purpose of being monitored
                    and/or controlled.";

                uses or-teiv-types:Top_Grp_Type;
                uses or-teiv-types:Origin_Entity_Mapping_Grp;
                key id;
            }
        }
    CreateStaticGroupPayloadExample:
      value:
        name: cell-filter-group-1
        type: static
        providedMembers:
          - o-ran-smo-teiv-ran:NRCellDU:
              - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
          - o-ran-smo-teiv-ran:GNBDUFunction:
              - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1
          - o-ran-smo-teiv-oam:ManagedElement:
              - id: urn:3gpp:dn:ManagedElement=1
          - o-ran-smo-teiv-ran:GNBDUFUNCTION_PROVIDES_NRCELLDU:
              - id: urn:o-ran:smo:teiv:sha512:GNBDUFUNCTION_PROVIDES_NRCELLDU=EA8BF964B4888BFD1991D8E2ECDFA7723118D3829C1378ACBB5484F9ADE328957641013EDF2BEC80CB8E4E0A46CC2D85B960EF25ABF61CC8601095948E368624
          - o-ran-smo-teiv-rel-oam-ran:MANAGEDELEMENT_MANAGES_GNBDUFUNCTION:
              - id: urn:o-ran:smo:teiv:sha512:MANAGEDELEMENT_MANAGES_GNBDUFUNCTION=86084B5A80FAC7339117CEB91A4838FAC28C50AF00C9A13DF66FFA497356A8F440626A935B9621D4C833F0A6DE2722EDC9A312E506D80235A8C1BF54D8DFACC8
    CreateDynamicGroupGetEntitiesByDomainPayloadExample:
      value:
        name: cell-filter-group-2
        type: dynamic
        criteria:
          queryType: getEntitiesByDomain
          domain: RAN
          targetFilter: /NRCellDU/attributes(nCI)
          scopeFilter: /NRCellDU/attributes[@cellLocalId=1]
    GroupsResponseExample:
      value:
        items:
          - id: urn:o-ran:smo:teiv:group=123e4567-e89b-12d3-a456-426614174000
            name: cell-filter-group-1
            type: static
            providedMembers:
              href: /groups/urn:o-ran:smo:teiv:group=123e4567-e89b-12d3-a456-426614174000/provided-members
            members:
              href: /groups/urn:o-ran:smo:teiv:group=123e4567-e89b-12d3-a456-426614174000/members
          - id: urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000
            name: cell-filter-group-2
            type: dynamic
            members:
              href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/members
        self:
          href: /groups?offset=0&limit=500
        first:
          href: /groups?offset=0&limit=500
        prev:
          href: /groups?offset=0&limit=500
        next:
          href: /groups?offset=0&limit=500
        last:
          href: /groups?offset=0&limit=500
        totalCount: 2
    MembersResponseExample:
      value:
        items:
          - o-ran-smo-teiv-ran:NRCellDU:
              - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
          - o-ran-smo-teiv-ran:GNBDUFunction:
              - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1
          - o-ran-smo-teiv-oam:ManagedElement:
              - id: urn:3gpp:dn:ManagedElement=1
          - o-ran-smo-teiv-ran:GNBDUFUNCTION_PROVIDES_NRCELLDU:
              - id: urn:o-ran:smo:teiv:sha512:GNBDUFUNCTION_PROVIDES_NRCELLDU=EA8BF964B4888BFD1991D8E2ECDFA7723118D3829C1378ACBB5484F9ADE328957641013EDF2BEC80CB8E4E0A46CC2D85B960EF25ABF61CC8601095948E368624
          - o-ran-smo-teiv-rel-oam-ran:MANAGEDELEMENT_MANAGES_GNBDUFUNCTION:
              - id: urn:o-ran:smo:teiv:sha512:MANAGEDELEMENT_MANAGES_GNBDUFUNCTION=86084B5A80FAC7339117CEB91A4838FAC28C50AF00C9A13DF66FFA497356A8F440626A935B9621D4C833F0A6DE2722EDC9A312E506D80235A8C1BF54D8DFACC8
        self:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/members?offset=0&limit=500
        first:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/members?offset=0&limit=500
        prev:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/members?offset=0&limit=500
        next:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/members?offset=0&limit=500
        last:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/members?offset=0&limit=500
        totalCount: 5
    ProvidedMembersResponseExample:
      value:
        items:
          - o-ran-smo-teiv-ran:NRCellDU:
              - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
          - o-ran-smo-teiv-ran:GNBDUFunction:
              - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1
          - o-ran-smo-teiv-oam:ManagedElement:
              - id: urn:3gpp:dn:ManagedElement=1
          - o-ran-smo-teiv-ran:GNBDUFUNCTION_PROVIDES_NRCELLDU:
              - id: urn:o-ran:smo:teiv:sha512:GNBDUFUNCTION_PROVIDES_NRCELLDU=EA8BF964B4888BFD1991D8E2ECDFA7723118D3829C1378ACBB5484F9ADE328957641013EDF2BEC80CB8E4E0A46CC2D85B960EF25ABF61CC8601095948E368624
          - o-ran-smo-teiv-rel-oam-ran:MANAGEDELEMENT_MANAGES_GNBDUFUNCTION:
              - id: urn:o-ran:smo:teiv:sha512:MANAGEDELEMENT_MANAGES_GNBDUFUNCTION=86084B5A80FAC7339117CEB91A4838FAC28C50AF00C9A13DF66FFA497356A8F440626A935B9621D4C833F0A6DE2722EDC9A312E506D80235A8C1BF54D8DFACC8
        self:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/provided-members?offset=0&limit=500
        first:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/provided-members?offset=0&limit=500
        prev:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/provided-members?offset=0&limit=500
        next:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/provided-members?offset=0&limit=500
        last:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/provided-members?offset=0&limit=500
        totalCount: 5
    MergeProvidedMembersPayloadExample:
      value:
        operation: merge
        providedMembers:
          - o-ran-smo-teiv-ran:NRCellDU:
              - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
    RemoveProvidedMembersPayloadExample:
      value:
        operation: remove
        providedMembers:
          - o-ran-smo-teiv-ran:NRCellDU:
              - id: urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
    StaticGroupResponseExample:
      value:
        id: urn:o-ran:smo:teiv:group=123e4567-e89b-12d3-a456-426614174000
        name: cell-filter-group-1
        type: static
        providedMembers:
          href: /groups/urn:o-ran:smo:teiv:group=123e4567-e89b-12d3-a456-426614174000/provided-members
        members:
          href: /groups/urn:o-ran:smo:teiv:group=123e4567-e89b-12d3-a456-426614174000/members
    DynamicGroupResponseExample:
      value:
        id: urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000
        name: cell-filter-group-2
        type: dynamic
        criteria:
          queryType: getEntitiesByDomain
          domain: RAN
          targetFilter: /NRCellDU/attributes(nCI)
          scopeFilter: /NRCellDU/attributes[@cellLocalId=1]
        members:
          href: /groups/urn:o-ran:smo:teiv:group=550e8400-e29b-41d4-a716-446655440000/members
    UpdateGroupNamePayloadExample:
      value:
        name: cell-filter-group-5

```

---

## Part 2 — Template Guide (`TEMPLATE_GUIDE.md`)

# Generic Topology & Inventory Query rApp Template

> **Purpose**: Create an rApp that can query ANY endpoint from Topology & Inventory API  
> **Supports**: Entities, Relationships, Domains - any T&I endpoint  
> **Fill Time**: 2 minutes

---

## 📝 Template Format

Fill in these 4 fields:

```yaml
Domain: ________________           # e.g., RAN, EQUIPMENT, OAM, REL_OAM_RAN
Entity_or_Relationship_Type: _____ # e.g., NRCellDU, GNBDUFunction, GNBDUFUNCTION_PROVIDES_NRCELLDU
Purpose: ________________          # What you're discovering/querying
T&I_API_URL: ________________      # Full T&I API path (from topology-inventory-api.yaml)
```

---

## 🎯 Examples

### Example 1: Query NRCellDU Entities
```yaml
Domain: RAN
Entity_or_Relationship_Type: NRCellDU
Purpose: Discover first 10 NRCellDUs from the network
T&I_API_URL: /domains/RAN/entity-types/NRCellDU/entities
```

### Example 2: Query GNB DU Functions
```yaml
Domain: RAN
Entity_or_Relationship_Type: GNBDUFunction
Purpose: List all GNB DU Functions in the RAN domain
T&I_API_URL: /domains/RAN/entity-types/GNBDUFunction/entities
```

### Example 3: Query Relationships
```yaml
Domain: RAN
Entity_or_Relationship_Type: GNBDUFUNCTION_PROVIDES_NRCELLDU
Purpose: Get relationships between GNBDUFunction and NRCellDU
T&I_API_URL: /domains/RAN/relationship-types/GNBDUFUNCTION_PROVIDES_NRCELLDU/relationships
```

### Example 4: Query All RAN Entities
```yaml
Domain: RAN
Entity_or_Relationship_Type: All RAN Entities
Purpose: Get all entities in RAN domain with filters
T&I_API_URL: /domains/RAN/entities
```

### Example 5: Query Equipment Entities
```yaml
Domain: EQUIPMENT
Entity_or_Relationship_Type: AntennaCapability
Purpose: Discover antenna capabilities
T&I_API_URL: /domains/EQUIPMENT/entity-types/AntennaCapability/entities
```

### Example 6: Query Specific Entity by ID
```yaml
Domain: RAN
Entity_or_Relationship_Type: NRCellDU
Purpose: Get specific NRCellDU entity details
T&I_API_URL: /domains/RAN/entity-types/NRCellDU/entities/{entityId}
```

### Example 7: Query Entity Relationships
```yaml
Domain: RAN
Entity_or_Relationship_Type: NRCellDU_Relationships
Purpose: Get all relationships for a specific NRCellDU
T&I_API_URL: /domains/RAN/entity-types/NRCellDU/entities/{entityId}/relationships
```

### Example 8: Query All Domains
```yaml
Domain: ALL
Entity_or_Relationship_Type: Domains
Purpose: List all available topology domains
T&I_API_URL: /domains
```

---

## 📚 T&I API URL Reference

### Entity Queries
```
/domains                                                               # Get all domains
/domains/{domain}/entity-types                                         # Get all entity types in domain
/domains/{domain}/entity-types/{entityType}/entities                   # Get entities of type
/domains/{domain}/entity-types/{entityType}/entities/{entityId}        # Get specific entity
/domains/{domain}/entities                                             # Get all entities in domain
```

### Relationship Queries
```
/domains/{domain}/relationship-types                                                      # Get all relationship types
/domains/{domain}/relationship-types/{relationshipType}/relationships                     # Get relationships of type
/domains/{domain}/relationship-types/{relationshipType}/relationships/{relationshipId}    # Get specific relationship
/domains/{domain}/entity-types/{entityType}/entities/{entityId}/relationships             # Get relationships for entity
```

### Common Domains
- `RAN` - Radio Access Network entities
- `EQUIPMENT` - Physical equipment entities
- `OAM` - Operations & Maintenance entities
- `REL_OAM_RAN` - Relationships between OAM and RAN
- `REL_EQUIPMENT_RAN` - Relationships between Equipment and RAN
- `TEIV` - Topology & Inventory base entities

### Common Entity Types (RAN Domain)
- `NRCellDU` - 5G NR Cell DU
- `NRCellCU` - 5G NR Cell CU
- `GNBDUFunction` - gNodeB DU Function
- `GNBCUFunction` - gNodeB CU Function
- `GNBCUUPFunction` - gNodeB CU UP Function
- `NRSectorCarrier` - NR Sector Carrier
- `ENodeBFunction` - eNodeB Function (4G)
- `EUtranCell` - E-UTRAN Cell (4G)

### Common Relationship Types
- `GNBDUFUNCTION_PROVIDES_NRCELLDU`
- `NRCELLDU_USES_NRSECTORCARRIER`
- `MANAGEDELEMENT_MANAGES_GNBDUFUNCTION`

---

## 🔧 Query Parameters Support

Your rApp will automatically support these query parameters:

### For List Queries (entities, relationships)
- `limit` (default: 10, max: 100) - Number of results
- `offset` (default: 0) - Pagination offset
- `targetFilter` (optional) - Filter what to return
- `scopeFilter` (optional) - Filter conditions

### Examples
```
GET /query?limit=20                                    # Get 20 results
GET /query?limit=10&offset=10                          # Get next page
GET /query?targetFilter=/sourceIds                     # Only return sourceIds
GET /query?scopeFilter=/NRCellDU/attributes[@nCI=123]  # Filter by nCI value
```

---

## 📤 Response Format

All responses normalized to:

```json
{
  "queryType": "NRCellDU",           // or relationship type, or "entities", "domains"
  "domain": "RAN",                   // the domain queried
  "count": 10,                       // number of results returned
  "totalCount": 100,                 // total available (from T&I)
  "results": [                       // normalized results array
    {
      "id": "urn:3gpp:dn:...",
      "type": "o-ran-smo-teiv-ran:NRCellDU",
      "attributes": {...},
      "sourceIds": [...],
      "metadata": {...}
    }
  ],
  "pagination": {                    // pagination info
    "self": "/query?limit=10&offset=0",
    "first": "/query?limit=10&offset=0",
    "next": "/query?limit=10&offset=10",
    "last": "/query?limit=10&offset=90"
  }
}
```

### For Relationships
```json
{
  "queryType": "GNBDUFUNCTION_PROVIDES_NRCELLDU",
  "domain": "RAN",
  "count": 5,
  "totalCount": 50,
  "results": [
    {
      "id": "urn:o-ran:smo:teiv:sha512:...",
      "type": "o-ran-smo-teiv-ran:GNBDUFUNCTION_PROVIDES_NRCELLDU",
      "aSide": "urn:3gpp:dn:...",     // A-side entity
      "bSide": "urn:3gpp:dn:...",     // B-side entity
      "sourceIds": [...],
      "metadata": {...}
    }
  ],
  "pagination": {...}
}
```

### For Single Entity by ID
```json
{
  "queryType": "NRCellDU",
  "domain": "RAN",
  "count": 1,
  "result": {                         // singular "result" for single entity
    "id": "urn:3gpp:dn:...",
    "type": "o-ran-smo-teiv-ran:NRCellDU",
    "attributes": {...},
    "sourceIds": [...],
    "metadata": {...}
  }
}
```

---

## 🎨 Advanced: URL with Path Parameters

If your URL contains `{entityId}` or `{relationshipId}`, the rApp will:
1. Expose it as a path parameter in the API
2. Substitute it when calling T&I

### Example
**Template Input**:
```yaml
T&I_API_URL: /domains/RAN/entity-types/NRCellDU/entities/{entityId}
```

**Generated API**:
```
GET /query/{entityId}
```

**Usage**:
```bash
curl http://rapp:8050/query/urn:3gpp:dn:ManagedElement=1,GNBDUFunction=1,NRCellDU=1
```

---

## 🚀 Complete Example

### Input to Template
```yaml
Domain: RAN
Entity_or_Relationship_Type: NRCellDU
Purpose: Discover first 10 NRCellDUs from the network
T&I_API_URL: /domains/RAN/entity-types/NRCellDU/entities
```

### Generated rApp Name
`ran-nrcelldu-query` (auto-generated from Domain + EntityType)

### Generated API Endpoints

**Main Query Endpoint**:
```
GET /query?limit=10&offset=0
```

**Response** (200 OK):
```json
{
  "queryType": "NRCellDU",
  "domain": "RAN",
  "count": 10,
  "totalCount": 150,
  "results": [
    {
      "id": "urn:3gpp:dn:SubNetwork=Europe,SubNetwork=Ireland,MeContext=NR03gNodeBRadio00030,ManagedElement=NR03gNodeBRadio00030,GNBDUFunction=1,NRCellDU=NR03gNodeBRadio00030-4",
      "type": "o-ran-smo-teiv-ran:NRCellDU",
      "attributes": {
        "cellLocalId": 4,
        "nCI": 12345,
        "nRPCI": 100,
        "nRTAC": 1
      },
      "sourceIds": [
        "urn:3gpp:dn:SubNetwork=Europe,SubNetwork=Ireland,MeContext=NR03gNodeBRadio00030,ManagedElement=NR03gNodeBRadio00030,GNBDUFunction=1,NRCellDU=NR03gNodeBRadio00030-4"
      ],
      "metadata": {
        "reliabilityIndicator": "OK",
        "firstDiscovered": "2025-01-07T12:20:12.245Z",
        "lastModified": "2025-01-08T10:40:36.461Z"
      }
    },
    ...9 more...
  ],
  "pagination": {
    "self": "/query?limit=10&offset=0",
    "first": "/query?limit=10&offset=0",
    "next": "/query?limit=10&offset=10",
    "last": "/query?limit=10&offset=140"
  }
}
```

**Standard Endpoints** (always included):
```
GET /health/liveness          # Kubernetes liveness probe
GET /health/readiness         # Kubernetes readiness probe
GET /metrics                  # Prometheus metrics
```

---

## 🔍 What the rApp Does Internally

1. **Receives request**: `GET /query?limit=10`
2. **Constructs T&I URL**: `{IAM_BASE_URL}/topology-inventory/v1alpha11/domains/RAN/entity-types/NRCellDU/entities?limit=10`
3. **Authenticates**: Uses OAuth to get token
4. **Calls T&I API**: With proper headers and authentication
5. **Parses response**: Extracts items, totalCount, pagination
6. **Normalizes**: Converts to standard format
7. **Returns**: JSON response

---

## ⚙️ Auto-Generated Features

### Infrastructure (100% Reusable)
✅ OAuth2 authentication with IAM  
✅ mTLS logging to platform  
✅ Health checks (liveness/readiness)  
✅ Prometheus metrics  
✅ Retry logic with exponential backoff  
✅ Error handling  

### Kubernetes Deployment
✅ Deployment manifest  
✅ Service (ClusterIP)  
✅ Service Account with RBAC  
✅ Network policies  
✅ ConfigMap  
✅ Helm chart  

### Security
✅ Non-root user (UID 60577)  
✅ Read-only root filesystem  
✅ No privilege escalation  
✅ Required RBAC role: `TopologyInventory_Application_ReadOnly`  

### Observability
✅ Structured JSON logging  
✅ Custom metrics:
  - `ti_query_requests_total` - Total queries
  - `ti_query_errors_total` - Failed queries
  - `ti_query_duration_seconds` - Query latency histogram
  - `ti_results_returned_total` - Results returned

---

## 📋 Submission Format

```
Create a Generic Topology & Inventory Query rApp:

Domain: [Your Domain]
Entity_or_Relationship_Type: [Your Type]
Purpose: [Your Purpose Statement]
T&I_API_URL: [Full T&I API Path]
```

---

## 📖 Real-World Use Cases

### Use Case 1: Cell Discovery
```yaml
Domain: RAN
Entity_or_Relationship_Type: NRCellDU
Purpose: Discover first 10 NRCellDUs from the network
T&I_API_URL: /domains/RAN/entity-types/NRCellDU/entities
```

### Use Case 2: Function Inventory
```yaml
Domain: RAN
Entity_or_Relationship_Type: GNBDUFunction
Purpose: Inventory all GNB DU Functions
T&I_API_URL: /domains/RAN/entity-types/GNBDUFunction/entities
```

### Use Case 3: Relationship Mapping
```yaml
Domain: RAN
Entity_or_Relationship_Type: GNBDUFUNCTION_PROVIDES_NRCELLDU
Purpose: Map which DU functions provide which cells
T&I_API_URL: /domains/RAN/relationship-types/GNBDUFUNCTION_PROVIDES_NRCELLDU/relationships
```

### Use Case 4: Equipment Discovery
```yaml
Domain: EQUIPMENT
Entity_or_Relationship_Type: AntennaCapability
Purpose: Discover all antenna capabilities in the network
T&I_API_URL: /domains/EQUIPMENT/entity-types/AntennaCapability/entities
```

### Use Case 5: Cross-Domain Relationships
```yaml
Domain: REL_OAM_RAN
Entity_or_Relationship_Type: MANAGEDELEMENT_MANAGES_GNBDUFUNCTION
Purpose: Find which managed elements manage which GNB functions
T&I_API_URL: /domains/REL_OAM_RAN/relationship-types/MANAGEDELEMENT_MANAGES_GNBDUFUNCTION/relationships
```

### Use Case 6: 4G Cell Discovery
```yaml
Domain: RAN
Entity_or_Relationship_Type: EUtranCell
Purpose: Discover E-UTRAN cells (4G)
T&I_API_URL: /domains/RAN/entity-types/EUtranCell/entities
```

---

## 🎯 Your Exact Use Case

```
Create a Generic Topology & Inventory Query rApp:

Domain: RAN
Entity_or_Relationship_Type: NRCellDU
Purpose: Discover first 10 NRCellDUs from the network
T&I_API_URL: /domains/RAN/entity-types/NRCellDU/entities
```

**Result**: Production-ready rApp that queries T&I for NRCellDUs with normalized responses!

---

## 💡 Pro Tips

### Tip 1: Use targetFilter for Performance
```
GET /query?targetFilter=/sourceIds;/attributes(nCI,cellLocalId)
```
Only returns specific fields, faster response

### Tip 2: Use scopeFilter for Conditional Queries
```
GET /query?scopeFilter=/NRCellDU/attributes[@cellLocalId>10]
```
Filter results based on attribute values

### Tip 3: Pagination for Large Datasets
```
GET /query?limit=50&offset=0    # First page
GET /query?limit=50&offset=50   # Second page
```

### Tip 4: Check Available Entity Types First
```yaml
T&I_API_URL: /domains/RAN/entity-types
```
Discover what entity types are available

### Tip 5: Explore Relationships
```yaml
T&I_API_URL: /domains/RAN/relationship-types
```
See what relationships exist in the domain

---

## ⏱️ Time to Working rApp

| Step | Time |
|------|-----:|
| Fill template (4 fields) | 1 min |
| Generate rApp | < 1 min |
| Deploy to K8s | 2 min |
| Test queries | 1 min |
| **Total** | **< 5 min** |

---

## 🎁 What Makes This Template Special

### Compared to Original NRDUCells App:
| Feature | Original | This Template |
|---------|----------|---------------|
| **Entity Types** | NRCellDU only | ANY entity type |
| **Relationships** | Not supported | ✅ Fully supported |
| **Domains** | RAN only | ALL domains |
| **URL Flexibility** | Hardcoded | ✅ Configurable |
| **Response Format** | T&I format | ✅ Normalized |
| **Query Parameters** | Limited | ✅ Full T&I params |

### This is a **TRUE GENERIC T&I CLIENT** that works with:
✅ Any domain (RAN, EQUIPMENT, OAM, etc.)  
✅ Any entity type (NRCellDU, GNBDUFunction, etc.)  
✅ Any relationship type  
✅ Single entity queries  
✅ List queries  
✅ Filtered queries  
✅ Paginated queries  

---

**Template Version**: 2.0.0  
**Type**: Generic Topology & Inventory Query Client  
**Based On**: Network Data Template App v2.0.3-0  
**T&I API Version**: v1alpha11


