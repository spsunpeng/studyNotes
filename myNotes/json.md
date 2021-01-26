json

- ObjectNode: put/set/setAll
  - string: put
  - objectNode: set/setAll , 那么，object如何转objectNode
  - arrayNode：set , 那么，array如何转arrayNode
  - list/Map/object：先转成writeValueAsString，再put
- JsonNode: get
- objectNode->JsonNode: JsonNode jsonNode2 = objectMapper.readTree(objectNode.toString());
- JsonNode->objectNode: setAll

```java
package com.example.json;

import com.example.json.model.UserModel;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.ObjectReader;
import com.fasterxml.jackson.databind.node.ArrayNode;
import com.fasterxml.jackson.databind.node.ObjectNode;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.TreeMap;

@SpringBootTest
class JsonApplicationTests {

    @Autowired
    private UserModel user;

    private static ObjectMapper objectMapper = new ObjectMapper();

    @Test
    void objectToString() {
        System.out.println(user.toString());
    }

    @Test
    void objectToJson() throws JsonProcessingException {
        String str = objectMapper.writeValueAsString(user);
        System.out.println(str);
    }

    @Test
    void objectNodeTest() throws JsonProcessingException {
        //普通、对象、数组、map
        ObjectNode objectNode = objectMapper.createObjectNode();
        objectNode.put("name", "xiaoMing");
        objectNode.put("age", 12);

        ObjectNode objectNode1 = objectMapper.createObjectNode();
        objectNode1.put("nainji", 3);
        objectNode1.put("banji", 2);

        objectNode.set("xuexiao", objectNode1);//{"name":"xiaoMing","age":12,"xuexiao":{"nainji":3,"banji":2}}
        //objectNode.setAll(objectNode1); //{"name":"xiaoMing","age":12,"nainji":3,"banji":2}

        ArrayNode arrayNode = objectMapper.createArrayNode();
        arrayNode.add("abc");
        arrayNode.add("cde");
        System.out.println(arrayNode);
        objectNode.set("arrayNode", arrayNode); //{"name":"xiaoMing","age":12,"xuexiao":{"nainji":3,"banji":2},"arrayNode":["abc","cde"]}

        System.out.println(objectNode);


        ArrayList<String> arrayList = new ArrayList<>();
        arrayList.add("abc");
        arrayList.add("cde");
        System.out.println(arrayList);
        String strList = objectMapper.writeValueAsString(arrayList);

        Map<String, String> hashMap = new HashMap<>();
        hashMap.put("001", "xiaoming");
        hashMap.put("002", "xiaoxinag");
        System.out.println(hashMap);
        String strHashMap = objectMapper.writeValueAsString(hashMap);

        Map<String, String> treeMap = new TreeMap<>();
        treeMap.put("101", "jia");
        treeMap.put("102", "yi");
        System.out.println(treeMap);
        String strTreeMap = objectMapper.writeValueAsString(treeMap);

        ObjectNode objectNode2 = objectMapper.createObjectNode();
        objectNode2.put("strList", strList);
        objectNode2.put("strHashMap", strHashMap);
        objectNode2.put("strTreeMap", strTreeMap);
        System.out.println(objectNode2);

        JsonNode jsonNode = objectNode;
        System.out.println(jsonNode);

        JsonNode jsonNode1 = objectNode2;
        System.out.println(jsonNode1);

        objectNode.setAll(objectNode2);
        System.out.println(objectNode);

        String str = objectNode.toString();

        JsonNode jsonNode2 = objectMapper.readTree(str);
        System.out.println(jsonNode2.get("arrayNode"));
    }


}
```