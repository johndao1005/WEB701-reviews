# WEB701-Evaluate

The main purpose of this milestone is to understand the purpose of the framework and to compare different web frameworks available through practical practice. With only two prototypes set up and running, It is hard to tell any significant difference in the appearance between frameworks as the design and layout can be transferred and tailored to match the design. When it comes to syntax and coding, React would be more simple than Svelte due to past experience as well as the use of javascript throughout the application development but Svelte also bring some familiar aspects such as the similar setup and style of simple HTML and CSS while also provide some build-in library and functions which React require a third party or community library to support the function of the framework.

Here is an example of the same component in React and Svelte :

Svelte :

```html
<script>
  import { onMount } from 'svelte';
  import axios from 'axios';
  import { useNavigate, useLocation } from "svelte-navigator";
  import { user } from "../../store/store";

  const navigate = useNavigate();
  const location = useLocation();

  let email = "john.doe@gmail.com";
  let name = "John Doe";
  let password;
  let phone;
  let address;
  let edit = false;
  let error = '';

  let id = localStorage.getItem('_id')
  onMount(async () => {
    getData();
	});
  

  const getData = async () => {
    try {
      const config = {
        headers: { "Access-Control-Allow-Origin": "*" },
      };
      const { data } = await axios.get(
        `http://localhost:5000/user/profile/${id}`,
        config
      );
      console.log(data);
      email = (data.email);
      name =(data.name);
      address =(data.address || "");
      phone =(data.phone || "");
    } catch (error) {
      error = (error);
    }
  };

  const handleLogout = () => {
    navigate("/", { replace: true });
    localStorage.removeItem("_id");
  };

  const handleUpdate = async () => {
    try {
      const config = {
        headers: { "Access-Control-Allow-Origin": "*" },
      };
      console.log(email, address, phone, name, password);
      const { data } = await axios.put(
        `http://localhost:5000/user/profile/${id}`,
        {
          email,
          address,
          phone,
          name,
          password,
        },
        config
      );
      console.log(data);

      if (data) {
        edit = false;
        getData();
      } else {
      }
    } catch (e) {
      error = e;
    }
  };

  $: if ($user) {
    navigate("/login", {
      state: { from: $location.pathname },
      replace: true,
    });
  }
</script>

<div class="row header">
	<h3>Welcome {name}</h3>
	<button on:click|preventDefault={()=>edit = !edit} >Edit details</button>
	<button on:click|preventDefault={()=>handleLogout()} >Log out</button>
</div>

{#if !$user}
  <div >
	<form class="column form" on:submit|preventDefault={handleUpdate}>
		<input 
		bind:value={name} 
		type="text" 
		name="name"
		disabled={!edit}
		placeholder="Username" />
		<br />
		<input 
		bind:value={email} 
		type="email" 
		disabled={!edit}
		name="email" 
		placeholder="Email" />
		<br />
		<input
		  bind:value={password}
		  type="password"
		  name="password"
		  disabled={!edit}
		  placeholder="Password"
		/>
		<br />
		<input
		  bind:value={phone}
		  type="phone"
		  name="phone"
		  disabled={!edit}
		  placeholder="Phone"/>
		<br />
		<input
		  bind:value={address}
		  type="string"
		  name="address"
		  disabled={!edit}
		  placeholder="Address"
		/>
		{#if edit}
		<button type="submit">Update</button>
		{/if}
	  </form>
  </div>
{/if}

<style>
	.form {
		max-width: 400px;
		margin:  30px auto 0;
	}
	.header {
		margin: 30px auto 0;
		justify-content: space-around;
	}
</style>
```

React :

```javascript
import React, { useEffect, useState } from "react";
import { Form, Input, Button, Typography,  Col,  } from "antd";
import { useDispatch, useSelector } from "react-redux";
import {
  updateUserProfile,
} from "../../store/action/authenticate.action";

/* eslint-disable no-template-curly-in-string */
const {Title} = Typography

const validateMessages = {
  required: "${label} is required!",
  types: {
    email: "${label} is not a valid email!",
    number: "${label} is not a valid number!",
  },
  number: {
    range: "${label} must be between ${min} and ${max}",
  },
};
/* eslint-enable no-template-curly-in-string */

const UserDetail = () => {
  const dispatch = useDispatch();
  const userLogin = useSelector((state) => state.userLogin);
  const { userInfo } = userLogin;

  const [form] = Form.useForm();
  const [name, setName] = useState(userInfo?.name);
  const [email, setEmail] = useState(userInfo?.email);
  const [address, setAddress] = useState(userInfo?.address);
  const [phone, setPhone] = useState(userInfo?.phone);
  useEffect(() => {
    if (!userInfo) {
      navigator("/login");
    } else {
      form.setFieldsValue({
        name: name,
        email: email,
        address: address,
        phone: phone,
      });
    }
  }, [dispatch, navigator, userInfo]);

  const submitHandler = () => {
    dispatch(updateUserProfile({ _id: userInfo._id, name, email, address, phone }));
  };

  const detailForm = () =>{
    return (
      <Col xs={24} sm={24} md={24} lg={18} xl={18}>
      <Form
      style={{maxWidth: 500}}
        form={form}
        layout={"vertical"}
        name="nest-messages"
        onFinish={submitHandler}
        validateMessages={validateMessages} 
      >
        <Form.Item
          name={"name"}
          label="Name"
          rules={[
            {
              required: true,
            },
          ]}
        >
          <Input value={"email"} onChange={(e) => setName(e.target.value)} />
        </Form.Item>
        <Form.Item
          name={"email"}
          label="Email"
          rules={[
            {
              type: "email",
              required: true,
            },
          ]}
        >
          <Input value={"email"} onChange={(e) => setEmail(e.target.value)} />
        </Form.Item>
        <Form.Item name={"address"} label="Address">
          <Input value={address} onChange={(e) => setAddress(e.target.value)} />
        </Form.Item>
        <Form.Item name={"phone"} label="Phone number">
          <Input value={phone} onChange={(e) => setPhone(e.target.value)} />
        </Form.Item>
        <Form.Item wrapperCol={{ offset: 8 }}>
          <Button type="primary" htmlType="submit">
            Update
          </Button>
        </Form.Item>
      </Form>
      </Col>)
  }

  return (
    <>
    <Title level={4}> User Details</Title>
    {detailForm()}
    </>
  );
};

export default UserDetail;
```

In my opinion, the syntax for Svelte is simple and practical compared to React while they provide the same outcome. Due to the market demand and limited time budget, React is still my final choice for the project but I believe I will come back to Svelte in the future.

